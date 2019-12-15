---
layout: content
---

# apk的拆包重构以及反编
<br/>
## 包体拆包重构

### 工具下载
1. apktool，用于包体的拆解与构建，从apk中提取图片和布局资源。[apktool下载](https://ibotpeaches.github.io/Apktool/install/)
2. dex2jar，将可运行文件classes.dex反编译为jar源码文件,版本选择上要选2.1，2.0以及2.0之前的不支持MultiDex。[dex2jar_2.1下载](https://github.com/pxb1988/dex2jar/releases)
3. jd-gui，查看jar源码文件。[jd-gui下载](https://github.com/java-decompiler/jd-gui/releases)

### 工具安装

apktool安装

1. 根据apktool网站提示的下载方法，右击wrapper script，链接存储为apktool，不要带拓展名
2. 下载apktool.jar，选择第一个下载最新版本
3. 把apktool_2.3.3.jar重命名为apktool.jar，然后把apktool.jar和apktool一起拷贝到/usr/local/bin路径下
4. 打开终端，输入apktool命令，看到相应输出及为安装成功

dex2jar && jd-gui
* 下载完dex2jar和 jd-gui解压一下就可以了，复制到工作目录方便操作。（顺便对dex2jar提一下权限，不然会报权限问题）

### 拆包与重构
1.终端输入cd path..... 进入到测试apk所在目录 

2.test.apk 将包体拆解

> apktool.bat d -o <output_dir> 

3.根据各自需求，对文件夹内的内容进行替换和修改 

4.将包体重构，此时包体未重签过，所以是安装不了的。

> apktool.bat b -o <output.apk> <input_dir>

### 包体重签

1. 将签名文件移植到apk目录下，方便操作
2. 确保机器上安装了java环境
3. 终端进入apk目录下，执行命令：

> jarsigner -verbose -keystore [your_key_store_path] -signedjar [signed_apk_name] [usigned_apk_name] [your_key_store_alias] -digestalg SHA1 -sigalg MD5withRSA -storepass [StorePass]

****字段说明:****

* [your_key_store_path]：密钥所在位置的路径
* [signed_apk_name]：签名后安装包名称
* [usigned_apk_name]：未签名的安装包名称
* [your_key_store_alias]：密钥的别名
* [StorePass]：keystore的密码

重签之后的apk包就是可以用的了

## java代码的抽取
代码的抽取只是一句命令行：
> sh dex2jar-2.1/d2j-dex2jar.sh apk_path

这里着重说下运行过程中遇到的问题：
### 问题一:资源包超两个G
报错如下:
```java
Exception in thread "main" java.lang.OutOfMemoryError: Required array size too large
    at java.nio.file.Files.readAllBytes(Files.java:3156)
    at com.googlecode.dex2jar.tools.Dex2jarCmd.doCommandLine(Dex2jarCmd.java:108)
    at com.googlecode.dex2jar.tools.BaseCmd.doMain(BaseCmd.java:290)
    at com.googlecode.dex2jar.tools.Dex2jarCmd.main(Dex2jarCmd.java:33)
```
出现原因: Files.readAllBytes 方法最大支持 Integer.MAX_VALUE - 8 大小的文件，也就是最大2GB的文件。一旦超过了这个限度，java 原生的方法就不能直接使用了。所以伴随这个报错的往往是游戏闪退。

解决办法: 把文件分成多个子区域分多次读取，这就会有多种方法可以使用。

### 问题二:java堆空间溢出
报错如下：
```java
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
    at java.nio.file.Files.read(Files.java:3099)
    at java.nio.file.Files.readAllBytes(Files.java:3158)
    at com.googlecode.dex2jar.tools.Dex2jarCmd.doCommandLine(Dex2jarCmd.java:108)
    at com.googlecode.dex2jar.tools.BaseCmd.doMain(BaseCmd.java:290)
    at com.googlecode.dex2jar.tools.Dex2jarCmd.main(Dex2jarCmd.java:33)
```
出现原因: 运行的java程序需要较大的内存时，可能会造成堆空间溢出。

解决办法: 将dex2jar-2.1/d2j-dex2jar.sh下的Xmx1024m改为Xmx4096m。Xmx表示jvm所需最大内存,这里我们可以根据实际需要自己调整设置。

即：
```
java -Xms512m -Xmx1024m -classpath "${_classpath}" "com.googlecode.dex2jar.tools.Dex2jarCmd" "$@"
```
改为：
```
java -Xms512m -Xmx4096m -classpath "${_classpath}" "com.googlecode.dex2jar.tools.Dex2jarCmd" "$@"
```

### 问题三:dex2jar不支持MultiDex
出现原因: dex2jar版本为v2.1之前的版本,那么dex2jar就会默认转化第一个dex文件而忽略其他dex文件. 2.1版本开始支持multidex,会直接执行 

解决办法: 下载2.1版本即可

shell脚本如下，只需将.sh与apk放在一起即可，具体操作请看注释：
```sh
#!/bin/sh
#默认填入数据，不填可以不用管

# dex=1,拆解包体，需付上apk路径；
# dex=2，重构包体，需付上keystore的文件名，Alias，签名密码，鉴于每个项目只有一个keystore，所以也可以写死在文件内；
# dex=3，反编译java代码，这里会生成.jar文件，可以方便程序复查代码

dex=None
oldApkPath=None;
keystore_name=None;
Alias=None;
StorePass=None;

if [ "$1" ]
then
dex="$1"
fi

if [ "$2" ]
then
    if [ ${dex} = 2 ] ; then 
        keystore_name="$2"
        echo "keystore_name~~~~~~~"
    else
        oldApkPath="$2"
        echo "oldApkPath~~~~~~~"
    fi
fi

if [ "$3" ]
then
Alias="$3"
fi

if [ "$4" ]
then
StorePass="$4"
fi


#当前apk的绝对或相对路径
APK_PATH=${oldApkPath}
#获取当前文件所在的绝对路径
SHELL_FOLDER=$(cd "$(dirname "$0")";pwd)
#操作类型
DEX=${dex}

echo "APK_PATH:", ${APK_PATH}
echo "SHELL_FOLDER:", ${SHELL_FOLDER}
echo "dex:", ${DEX}
time=$(date "+%Y_%m_%d_%H_%M_%S")
echo "${time}"


if [ ${APK_PATH} = None -a ${DEX} = 1 ] ; then
echo "apk没传，退出～～～"
exit 0
fi

if [ ${keystore_name} = None -a ${DEX} = 2  ] ; then
echo "keystore_name没有，无法重签～～"
exit 0
fi

if [ ${Alias} = None -a ${DEX} = 2  ] ; then
echo "Alias无，无法重签～～～"
exit 0
fi

if [ ${StorePass} = None -a ${DEX} = 2  ] ; then
echo "签名密码没有，无法重签～～～"
exit 0
fi

if [ -x  "usigned" -a ${DEX} = 1 ]
then
echo "Remove file usigned"
rm -rf "usigned"
fi

if [ -x  "usigned.apk" -a ${DEX} = 2 ]
then
echo "Remove file usigned.apk"
rm -rf "usigned.apk"
fi

if [ ${DEX} = 1 ]; then
echo "开始拆解包体~~~"
apktool d -o usigned ${APK_PATH}
fi

if [ ${DEX} = 2 ]; then
echo "开始重构包体~~~"
apktool b -o "usigned.apk" ${SHELL_FOLDER}"/usigned"
echo "开始重签apk包体～～～～"
jarsigner -verbose -keystore ${keystore_name} -signedjar ${time}"_resign.apk" usigned.apk ${Alias} -digestalg SHA1 -sigalg MD5withRSA -storepass ${StorePass}
fi

if [ ${DEX} = 3 ]; then
echo "开始解析包体内的Java代码～～"
sh dex2jar-2.1/d2j-dex2jar.sh ${APK_PATH}
fi

```
