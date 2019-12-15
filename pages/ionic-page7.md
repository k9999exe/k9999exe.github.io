---
layout: content
---

# Angularjs中对时间格式：/Date(1448864369815)/ 的处理
<br/>
我们在后台对数据进行json序列化时，如果数据中包含有日期，序列化后返回到前端的结果可能是这样的： /Date(1448864369815)/  。可是往往我们要在前台显示正常的日期格式，该如何处理呢？在angularjs（后面简称 ng）中处理起来还是挺方便的，首先我们来看下ng中内置的过滤器：date。

ng 过滤器有两种使用方式，分别是： ng表达式  和  控制器中使用代码调用

## ng表达式

{% raw %}
```html
<!-- 表达式中使用 -->
{{ dt1 | date:'yyyy-MM-dd HH:mm:ss' }}
```
{% endraw %}

## 控制器中使用
```js
//必须注入 $filter 模块
app.controller("demoCtrl", ["$scope", "$filter", function($scope, $filter){
    $scope.dt1 = new Date();

    //控制器中使用
    $scope.dt2 = $filter("date")($scope.dt1, "yyyy-MM-dd HH:mm:ss");
}]);
```

因为 $filter("date")(param, format) 中的 param 是可以接收时间戳的，所以如果我们的日期格式是： /Date(1448864369815)/ 这样的，我们只要获取到里面的时间戳（即： 1448864369815），就可以进行格式转换了，如下所示：

```js
//前台输出 dt3 结果为 2015-11-30 14：19：29
$scope.dt3 = $filter("date")(1448864369815, "yyyy-MM-dd HH:mm:ss");
```

那么我们如何获取 /Date(1448864369815)/  里面的时间戳  1448864369815  呢？方法如下：
```js
var _dt = "Date(1448864369815)";

//转成数字类型
var timestamp = Number(_dt.replace(/\/Date\((\d+)\)\//, "$1"));
```

在 ng 中是支持自定义过滤器的（自定义过滤器如果不了解，请自行百度），因此我们可以在原有的 date 过滤器的基础上进行一次包装，编写一个我们自己的过滤器，实现由  /Date(1448864369815)/  到常规时间格式的转换。代码如下：
```js
//自定义过滤器 jsonDate
app.filter("jsonDate", function($filter) {
   return function(input, format) {
        //先得到时间戳
        var timestamp = Number(input.replace(/\/Date\((\d+)\)\//, "$1"));

        //转成指定格式
        return $filter("date")(timestamp, format);
   } 
});
```

实现后，就可以像内置的 date 过滤器一样使用了。

```js
// 控制器中使用 
$filter("jsonDate")("Date(1448864369815)", "yyyy-MM-dd HH:mm:ss");
```
{% raw %}
```html
<!-- 表达式中使用 -->
{{"Date(1448864369815)" | jsonDate:"yyyy-MM-dd HH:mm:ss"}}
```
{% endraw %}