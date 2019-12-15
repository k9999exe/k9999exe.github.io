---
layout: content
---

# Angular中的异常机制与异常之外的处理 
<br/>
官方文档中提到了throw异常
```js
angular.module('exceptionOverride', []).factory('$exceptionHandler', function() {
  return function(exception, cause) {
    exception.message += ' (caused by "' + cause + '")';
    throw exception;
  };
});
```
捕获异常
```js
try { ... } catch(e) { $exceptionHandler(e); }
```

其实这两种在js异常处理中也经常用到，但是这和js中使用的有什么不同呢，下面我们就继续考中一下，其实在angluarjs中要处理异常就必须使用service服务中的$ExceptionHander组件，引入组件后，你就可以大胆的处理不想调试的bug了，下面举个例子：
```js

var module = angular.module("apple", []);

module = [ '$scope' , '$exceptionHandler' , function ( scope, exceptlogger) {....

try { 函数体} catch(e) {exceptlogger(e); }

```

在做项目的时候发现有如果是调用插件的方法出现错误，如uncaught exception等错误，这种捕获方式是无法捕获异常的，那么我们就应该使用另一种捕获异常的方式。

使用window.onerror的方法捕获异常：
```js
//捕获函数体内或函数体外的错误
window.onerror = fnErrorTrap;
function fnErrorTrap(msg,url){
    console.log( "Error: " + msg+ "URL: " + url);
    //注意下面的返回值true，表示浏览器不对这个错误进行处理，不会显示错误提示
    return true;
}
```