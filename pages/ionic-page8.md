---
layout: content
---

# IONIC WebApp之防止短时间内按钮多次点击
<br/>
因网络延迟的缘故，对着某个按钮狂点导致请求过多，刚开始想直接把按钮disabled掉，然后发觉这个按钮是div样式，并用的ng-click做的事件绑定，因而并不奏效。

```js

.config(['$provide',function($provide){
    //解决重复点击BUG
    $provide.decorator('ngClickDirective',['$delegate','$timeout',function($delegate,$timeout){
        var original = $delegate[0].compile;
        var delay = 500;
        $delegate[0].compile = function(element,attrs,transclude){
            var disabled = false;
            function onClick(evt){
                if(disabled){
                    evt.preventDefault();
                    evt.stopImmediatePropagation();
                }else{
                    disabled = true;
                    $timeout(function(){
                            disabled = false;
                    }, delay, false);
                }
            }
            element.on('click', onClick);
                return original(element, attrs, transclude);
            };
        return $delegate;
    }]);
}])

```