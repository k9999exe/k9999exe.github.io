---
layout: content
---

# Electron项目打包window启动程序

&nbsp;
## electron-packager 介绍

[electron-packager官方文档](https://github.com/electron/electron-packager)

electron-packager 命令行结构：

```
electron-packager <sourcedir> <appname> --platform=<platform> --arch=<arch> [optional flags...]
```

&nbsp;
## 安装插件

通过调用命令行安装的electron-packager来进行生成启动程序

> npm install electron-packager --save-dev

&nbsp;
## 打包命令实例

修改项目根目录下的配置文件 package.json

```script
"scripts": {
    "start": "electron .",
    "start2": "electron . --enable-logging",
    //开启主线程 log
	"electron_build": "electron-packager ./ helloworld --platform=win32 --arch=x64  --out=./app --app-version=1.0.0 --overwrite  --icon=./myicon.ico"
	// 注意事项 electron-packager ./ 会取当前目录下package.json文件（即当前文件）中 main 值指向的入口
  }
```

&nbsp;
## 运行打包命令

> npm run electron_build

&nbsp;
## 生成文件

在根目录下的app文件下会出现对应名称文件夹，双击exe即可运行程序

&nbsp;
## electron跨域问题

```js
// Create the browser window.
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
	  webSecurity: false
    }
  })

  // and load the index.html of the app.
  //mainWindow.loadURL('window.html')
  mainWindow.loadFile('https://www.baidu.com')
```

webSecurity是什么意思呢？顾名思义，他是设置web安全性，如果参数设置为 false，它将禁用相同地方的规则 (通常测试服), 并且如果有2个非用户设置的参数，就设置 allowDisplayingInsecureContent 和 allowRunningInsecureContent的值为true。 （webSecurity的默认值为true）

allowDisplayingInsecureContent表示是否允许一个使用 https的界面来展示由 http URLs 传过来的资源。默认false。
allowRunningInsecureContent表示是否允许一个使用 https的界面来渲染由 http URLs 提交的html，css，javascript。默认为 false。
