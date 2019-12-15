---
layout: content
---

# Angularjs Filter用法详解
<br/>

## Filter简介 

Filter是用来格式化数据用的。 

Filter的基本原型（ '\|' 类似于Linux中的管道模式）：
{% raw %}
```html
{{ expression | filter }} 
```
{% endraw %}

Filter可以被链式使用（即连续使用多个filter）：
{% raw %}
```html
{{ expression | filter1 | filter2 | ... }}
```
{% endraw %}

Filter也可以指定多个参数：
{% raw %}
```html
{{ expression | filter:argument1:argument2:... }}
```
{% endraw %}

## AngularJS内建的Filter
AngularJS内建了一些常用的Filter
### currencyFilter（currency）：
用途：格式化货币

方法原型：
```js
function(amount, currencySymbol, fractionSize)
```
用法：
{% raw %}
```html
{{ 12 | currency}}  <!--将12格式化为货币，默认单位符号为 '$', 小数默认2位-->

{{ 12.45 | currency:'￥'}} <!--将12.45格式化为货币，使用自定义单位符号为 '￥', 小数默认2位-->

{{ 12.45 | currency:'CHY￥':1}} <!--将12.45格式化为货币，使用自定义单位符号为 'CHY￥', 小数指定1位, 会执行四舍五入操作 -->

{{ 12.55 | currency:undefined:0}} <!--将12.55格式化为货币， 不改变单位符号， 小数部分将四舍五入 -->
```
{% endraw %}

### dateFilter（date）：
用途：格式化日期

方法原型：
```js
function(date, format, timezone)
```
用法：
{% raw %}
```html
<!--使用ISO标准日期格式 -->
{{ '2015-05-20T03:56:16.887Z' | date:"MM/dd/yyyy @ h:mma"}}

<!--使用13位（单位：毫秒）时间戳 -->
{{ 1432075948123 | date:"MM/dd/yyyy @ h:mma"}}

<!--指定timezone为UTC -->
{{ 1432075948123 | date:"MM/dd/yyyy @ h:mma":"UTC"}}
```
{% endraw %}

### filterFilter（filter）：
用途：过滤数组

方法原型：
```js
function(array, expression, comparator)
```
用法：
{% raw %}
```html
<div ng-init="myArr = [{name:'Tom', age:20}, {name:'Tom Senior', age:50}, {name:'May', age:21}, {name:'Jack', age:20}, {name:'Alice', age:22}]">
    <!-- 参数expression使用String，将全文搜索关键字 'a' -->
    <div ng-repeat="u in myArr | filter:'a' ">
        <p>Name:{{u.name}}</p>
        <p>Age:{{u.age}}</p>
        <br />
    </div>
</div>
```
{% endraw %}
用法2（参数expression使用function）：
{% raw %}
```html
 // 先在Controller中定义function: myFilter
 $scope.myFilter = function (item) {
     return item.age === 20;
 };
 
<div ng-repeat="u in myArr | filter:myFilter ">
    <p>Name:{{u.name}}</p>
    <p>Age:{{u.age}}</p>
    <br />
</div>
```
{% endraw %}

用法3（参数expression使用object）：
{% raw %}
```html
<div ng-init="myArr = [{name:'Tom', age:20}, {name:'Tom Senior', age:50}, {name:'May', age:21}, {name:'Jack', age:20}, {name:'Alice', age:22}]">
    <div ng-repeat="u in myArr | filter:{age: 21} ">
        <p>Name:{{u.name}}</p>
        <p>Age:{{u.age}}</p>
        <br />
     </div>
</div>
```
{% endraw %}

用法4（指定comparator为true或false）：
{% raw %}
```html
<div ng-init="myArr = [{name:'Tom', age:20}, {name:'Tom Senior', age:50}, {name:'May', age:21}, {name:'Jack', age:20}, {name:'Alice', age:22}]">
    Name:<input ng-model="yourName" />
    <!-- 指定comparator为false或者undefined，即为默认值可不传，将以大小写不敏感的方式匹配任意内容 -->
    <!-- 可以试试把下面代码的comparator为true，true即大小写及内容均需完全匹配 -->
    <div ng-repeat="u in myArr | filter:{name:yourName}:false ">
        <p>Name:{{u.name}}</p>
        <p>Age:{{u.age}}</p>
        <br />
    </div>
</div>
```
{% endraw %}

用法5（指定comparator为function）：
{% raw %}
```html
// 先在Controller中定义function:myComparator, 此function将能匹配大小写不敏感的内容，但与comparator为false的情况不同的是，comparator必须匹配全文
$scope.myComparator = function (expected, actual) {
    return angular.equals(expected.toLowerCase(), actual.toLowerCase());
}

<div ng-init="myArr = [{name:'Tom', age:20}, {name:'Tom Senior', age:50}, {name:'May', age:21}, {name:'Jack', age:20}, {name:'Alice', age:22}]">
    Name:<input ng-model="yourName" />
    <div ng-repeat="u in myArr | filter:{name:yourName}:myComparator ">
        <p>Name:{{u.name}}</p>
    <p>Age:{{u.age}}</p>
        <br />
</div>
</div>
```
{% endraw %}

### jsonFilter（json）：
方法原型：
```js
function(object, spacing)
```
用法（将对象格式化成标准的JSON格式）：
{% raw %}
```html
{{ {name:'Jack', age: 21} | json}}
```
{% endraw %}

### limitToFilter（limitTo）：
方法原型：
```js
function(input, limit)
```
用法（选取前N个记录）：
{% raw %}
```html
<div ng-init="myArr = [{name:'Tom', age:20}, {name:'Tom Senior', age:50}, {name:'May', age:21}, {name:'Jack', age:20}, {name:'Alice', age:22}]">
    <div ng-repeat="u in myArr | limitTo:2">
        <p>Name:{{u.name}}
        <p>Age:{{u.age}}
    </div>
</div>
```
{% endraw %}

### lowercaseFilter（lowercase）/uppercaseFilter（uppercase）：
方法原型：
```js
function(string)
```
用法：
{% raw %}
```html
China has joined the {{ "wto" | uppercase }}.
We all need {{ "MONEY" | lowercase }}.
```
{% endraw %}

### numberFilter（number）:
方法原型：
```js
function(number, fractionSize)
```
用法：
{% raw %}
```html
{{ "3456789" | number}}
<br />
{{ true | number}}
<br />
{{ 12345678 | number:1}}
```
{% endraw %}

### orderByFilter（orderBy）：
方法原型：
```js
function(array, sortPredicate, reverseOrder)
```
用法（选取前N个记录）：
{% raw %}
```html
<div ng-init="myArr = [{name:'Tom', age:20, deposit: 300}, {name:'Tom', age:22, deposit: 200}, {name:'Tom Senior', age:50, deposit: 200}, {name:'May', age:21, deposit: 300}, {name:'Jack', age:20, deposit:100}, {name:'Alice', age:22, deposit: 150}]">
    <!--deposit前面的'-'表示deposit这列倒叙排序，默认为顺序排序
    参数reverseOrder：true表示结果集倒叙显示-->
    <div ng-repeat="u in myArr | orderBy:['name','-deposit']:true">
        <p>Name:{{u.name}}</p>
        <p>Deposit:{{u.deposit}}</p>
        <p>Age:{{u.age}}</p>
        <br />
    </div>
</div>
```
{% endraw %}

### 自定义Filter
和Directive一样，如果内建的Filter不能满足你的需求，你当然可以定义一个专属于你自己的Filter。我们来做一个自己的Filter：capitalize_as_you_want，该Filter会使你输入的字符串中的首字母、指定index位置字母以及指定的字母全部大写。 

方法原型：
```js
function (input, capitalize_index, specified_char)
```
完整的示例代码：
{% raw %}
```html
<!DOCTYPE>
<html>
<head>
    <script src="/Scripts/angular.js"></script>
    <script type="text/javascript">
        (function () {
            var app = angular.module('ngCustomFilterTest', []);

            app.filter('capitalize_as_you_want', function () {
                return function (input, capitalize_index, specified_char) {
                    input = input || '';
                    var output = '';

                    var customCapIndex = capitalize_index || -1;

                    var specifiedChar = specified_char || '';

                    for (var i = 0; i < input.length; i++) {
                        // 首字母肯定是大写的， 指定的index的字母也大写
                        if (i === 0 || i === customCapIndex) {
                            output += input[i].toUpperCase();
                        }
                        else {
                            // 指定的字母也大写
                            if (specified_char != '' && input[i] === specified_char) {
                                output += input[i].toUpperCase();
                            }
                            else {
                                output += input[i];
                            }
                        }
                    }

                    return output;
                };
            });

        })();
    </script>
</head>
<body ng-app="ngCustomFilterTest">
    <input ng-model="yourinput" type="text">
    <br />
    Result: {{ yourinput | capitalize_as_you_want:3:'b' }}
</body>
</html>
```
{% endraw %}