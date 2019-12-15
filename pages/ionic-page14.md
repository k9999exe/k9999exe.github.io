---
layout: content
---

# JavaScript调用AngularJS的函数/$scope/变量
<br/>
使用背景：

需要在其他JavaScript文件中调用AngularJS内部方法或改变$scope变量，同时还要保持双向数据绑定；

首先获取AngularJS application：

## 方法一：通过controller来获取app
```js
var appElement = document.querySelector('[ng-controller=mainController]');
```

然后在获取$scope变量：

```js
var $scope = angular.element(appElement).scope(); 
```

如果改变了其中的变量之后，需要在页面表现出来，还需要调用apply()方法：

```js
$scope.$apply();
```

## 方法二：通过DOM操作获取app 
```js
//通过DOM操作获取app对象
var element = angular.element($document.getElementById("app"));
//得到app对象，可以获取app的controller
var controller = element.controller();
//得到app对象，可以获取app的$scope
var scope = element.scope();
//调用$scope中的方法
scope.sub1();
//调用方法后，可以重新绑定，在页面同步（可选）
scope.$apply();
//调用字段
scope.field1;
```

几个相关函数：
* 获取当前元素的$socpe:     angular.element(domElement).scope() to get the current scope for the element
* 获取当前app的injector：   angular.element(domElement).injector() to get the current app injector
* 获取当前元素的controller：angular.element(domElement).controller() to get a hold of the ng-controller instance.