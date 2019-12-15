---
layout: content
---

# Angularjs 事件指令
<br/>

## ngClick
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：单击</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <div ng-click="click()">click me</div>
  <button ng-click="click()">click me</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.click = function () {
    alert('click');
  }
});
```
<br/>

## ngDblclick
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：双击</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <div ng-dblclick="dblclick()">click me</div>
  <button ng-dblclick="dblclick()">click me</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.dblclick = function () {
    alert('click');
  }
});
```
<br/>

## ngBlur
* * *
<dl>
<dt>适用标签：</dt>
<dd>a</dd>
<dd>input</dd>
<dd>select</dd>
<dd>textarea</dd>
<dt>触发条件：失去焦点</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <a href="" ng-blur="blur()">link</a>
  <input type="text" ng-blur="blur()"/>
  <textarea cols="30" rows="10" ng-blur="blur()"></textarea>
  <select ng-blur="blur()">
    <option>----</option>
    <option>jacky</option>
    <option>rose</option>
  </select>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.blur = function () {
    alert('blur');
  }
});
```
<br/>

## ngFocus
* * *
<dl>
<dt>适用标签：</dt>
<dd>a</dd>
<dd>input</dd>
<dd>select</dd>
<dd>textarea</dd>
<dt>触发条件：获取焦点</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <a href="" ng-focus="focus()">link</a>
  <input type="text" ng-focus="focus()"/>
  <textarea cols="30" rows="10" ng-focus="focus()"></textarea>
  <select ng-focus="focus()">
    <option>----</option>
    <option>jacky</option>
    <option>rose</option>
  </select>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.focus= function () {
    alert('focus');
  }
});
```
<br/>

## ngChange
* * *
<dl>
<dt>适用标签：input</dt>
<dt>触发条件：model更新</dt>
</dl>

输入框的内容改变并不代表model的值更新。输入的内容是符合类型和校验条件的，model值会更新。

```html
<div ng-controller="LearnCtrl">
  <input type="text" 
    ng-model="text" 
    ng-change="change()"         
    ng-minlength="5"/>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  //$scope.text='';
  $scope.change = function () {
    alert('change');
  }            
});
```
<br/>

## ngCopy
* * *
<dl>
<dt>适用标签：</dt>
<dd>a</dd>
<dd>input</dd>
<dd>select</dd>
<dd>textarea</dd>
<dt>触发条件：复制。鼠标右键复制和快捷键Ctrl+C都会触发。</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-copy="copy()"/>
  <textarea cols="30" rows="10" ng-copy="copy()"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.copy = function () {
    alert('copy');
  }
});
```
<br/>

## ngCut
* * *
<dl>
<dt>适用标签：</dt>
<dd>a</dd>
<dd>input</dd>
<dd>select</dd>
<dd>textarea</dd>
<dt>触发条件：剪切。鼠标右键剪切和快捷键Ctrl+X都会触发。</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-cut="cut()"/>
  <textarea cols="30" rows="10" ng-cut="cut()"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.cut = function () {
    alert('cut');
  }
});
```
<br/>

## ngPaste
* * *
<dl>
<dt>适用标签：</dt>
<dd>a</dd>
<dd>input</dd>
<dd>select</dd>
<dd>textarea</dd>
<dt>触发条件：粘贴。鼠标右键粘贴和快捷键Ctrl+V都会触发。</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-paste="paste()"/>
  <textarea cols="30" rows="10" ng-paste="paste()"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.paste = function () {
    alert('paste');
  }
});
```
<br/>

## ngKeydown
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：键盘按键按下</dt>
</dl>

要把$event传过去，一般都是要判断按了哪个按键的。

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-keydown="keydown($event)"/>
  <textarea cols="30" rows="10" ng-keydown="keydown($event)"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.keydown = function ($event) {
    alert($event.keyCode);
  }
});
```
<br/>

## ngKeyup
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：键盘按键按下并松开</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-keyup="keyup($event)"/>
  <textarea cols="30" rows="10" ng-keyup="keyup($event)"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.keyup = function ($event) {
    alert($event.keyCode);
  }
});
```
<br/>

## ngKeypress
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：键盘按键按下</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-keypress="keypress($event)"/>
  <textarea cols="30" rows="10" ng-keypress="keypress($event)"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.keypress = function ($event) {
    alert($event.keyCode);
  }
});
```
<br/>

## ngKeyup
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：键盘按键按下并松开</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <input type="text" ng-keyup="keyup($event)"/>
  <textarea cols="30" rows="10" ng-keyup="keyup($event)"></textarea>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.keyup = function ($event) {
    alert($event.keyCode);
  }
});
```
<br/>

## keydown，keypress，keydown三者区别
* * *
### 引发事件的按键
非字符键不会引发 KeyPress 事件，但非字符键却可以引发 KeyDown 和 KeyUp 事件。 
### 事件引发的时间
KeyDown 和 KeyPress 事件在按下键时发生，KeyUp 事件在释放键时发生。
### 事件发生的顺序
KeyDown -> KeyPress -> KeyUp。 如果按一个键很久才松开，发生的事件为：KeyDown -> KeyPress -> KeyDown -> KeyPress -> KeyDown -> KeyPress -> ... -> KeyUp。

* KeyDown触发后，不一定触发KeyUp，当KeyDown 按下后，拖动鼠标，那么将不会触发KeyUp事件。
* KeyPress主要用来捕获数字(注意：包括Shift+数字的符号)、字母（注意：包括大小写）、小键盘等除了F1-12、SHIFT、Alt、Ctrl、Insert、Home、PgUp、Delete、End、PgDn、ScrollLock、Pause、NumLock、{菜单键}、{开始键}和方向键外的ANSI字符。
* KeyDown 和KeyUp 通常可以捕获键盘除了PrScrn所有按键(这里不讨论特殊键盘的特殊键）。
* KeyPress 只能捕获单个字符。
* KeyDown 和KeyUp 可以捕获组合键。
* KeyPress 可以捕获单个字符的大小写。
* KeyDown和KeyUp 对于单个字符捕获的KeyValue 都是一个值，也就是不能判断单个字符的大小写。
* KeyPress 不区分小键盘和主键盘的数字字符。
* KeyDown 和KeyUp 区分小键盘和主键盘的数字字符。
* 其中PrScrn 按键KeyPress、KeyDown和KeyUp 都不能捕获。

<br/>

## ngMousedown
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：鼠标按下，左右中间按下都会触发</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <button ng-mousedown="mousedown($event)">button</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.mousedown = function ($event) {
    alert($event.which);
  }
});
```
<br/>

## ngMouseup
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：鼠标按下弹起，左右中间按下弹起都会触发</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <button ng-mouseup="mouseup($event)">button</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.mouseup = function ($event) {
    alert($event.which);
  }
});
```
<br/>

## ngMouseenter
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：鼠标进入</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <button ng-mouseenter="mouseenter($event)">button</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.mouseenter = function ($event) {
    alert('mouseenter');
  }
});
```
<br/>

## ngMouseleave
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：鼠标离开</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <button ng-mouseleave="mouseleave($event)">button</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.mouseleave = function ($event) {
    alert('mouseleave');
  }
});
```
<br/>

## ngMousemove
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：鼠标移动</dt>
</dl>

```html
<div ng-controller="LearnCtrl">
  <button ng-mousemove="mousemove($event)">button</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.mousemove = function ($event) {
    alert('mousemove');
  }
});
```
<br/>

## ngMouseover
* * *
<dl>
<dt>适用标签：所有</dt>
<dt>触发条件：鼠标进入</dt>
</dl>
```html
<div ng-controller="LearnCtrl">
  <button ng-mouseover="mouseover($event)">button</button>
</div>
```
```js
angular.module('learnModule', [])
.controller('LearnCtrl', function ($scope) {
  $scope.mouseover = function ($event) {
    alert('mouseover');
  }
});
```