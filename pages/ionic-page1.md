---
layout: content
---

# IONIC-APP-LOADER

Ionic 热更新相关说明
[github原文章](https://github.com/markmarijnissen/cordova-app-loader).
<br/><br/>

## 逻辑简单整理
1.ionic项目每次启动时会调用bootstrap.js

2.bootstrap.js里比较当前manifest.json与服务器上的manifest.json进行比较

3.当服务器上的manifest.json里的某个文件值发生变化是，会通过filetransfer把当前文件下载，并更新本地manifest.json文件。

4.更新后会重新启动程序并重新调用bootstrap.js，当本地manifest.json与服务器上的manifest.json相同是会调用启动angular.bootstrap。
<br/><br/>
## 所需说明
1.ionic项目热更新需要cordova-plugin-file插件和cordova-plugin-file-transfer插件 

PS：因cordova-plugin-file插件在cordova-android7.0以后调用了ionic-plgin-webview。webview插件里对缓存地址进行了统一管理。android里需要加上_app_file_，比如：http://localhost/_app_file_/data/data/com.avicit.portal/files/files/test.png。ios里需要以ionic://localhost开头，比如ionic://localhost/data/data/com.avicit.portal/files/files/test.png

PS：因cordova-plugin-file-transfer以表明为Deprecated，在cordova-android7.0以后需要用XMLHttpRequest来把远程文件缓存到本地

2.bootstrap.js解析 

bootstrap.js分为4段 

第一段为请求文件内容部分

```js
// xhr placeholder to avoid using var, not to be used
window.pegasus = function pegasus(a, xhr) {
  xhr = new XMLHttpRequest();
  // Open url
  xhr.open('GET', a);
  // Reuse a to store callbacks
  a = [];
  // onSuccess handler
  // onError   handler
  // cb        placeholder to avoid using var, should not be used
  xhr.onreadystatechange = xhr.then = function(onSuccess, onError, cb, result) {
    // Test if onSuccess is a function or a load event
    if (onSuccess.call) a = [onSuccess, onError];
    // Test if request is complete
    if (xhr.readyState == 4) {
      try {
        if(xhr.status === 200 || xhr.status === 0){
          result = JSON.parse(xhr.responseText);
          cb = a[0];
        } else {
          result = new Error('Status: '+xhr.status);
          cb = a[1];
        }
      } catch(e) {
        result = e;
        cb = a[1];
      }
      // Safari doesn't support xhr.responseType = 'json'
      // so the response is parsed
      if (cb) cb(result);
    }
  };
  // Send
  xhr.send();
  // Return request
  return xhr;
};
//------------------------------------------------------------------
```
第二段为根据manifest.json内容加载所需要的文件

```js
// Step 2: After fetching manifest (localStorage or XHR), load it
function loadManifest(manifest,fromLocalStorage,timeout){
  // Safety timeout. If BOOTSTRAP_OK is not defined,
  // it will delete the 'localStorage' version and revert to factory settings.
  if(fromLocalStorage){
    setTimeout(function(){
      if(!window.BOOTSTRAP_OK){
        console.warn('BOOTSTRAP_OK !== true; Resetting to original manifest.json...');
        localStorage.removeItem('manifest');
        location.reload();
      }
    },timeout);
  }

  if(!manifest.load) {
    console.error('Manifest has nothing to load (manifest.load is empty).',manifest);
    return;
  }

  var el,
      head = document.getElementsByTagName('head')[0],
      scripts = manifest.load.concat(),
      now = Date.now();

  // Load Scripts
  function loadScripts(){
    scripts.forEach(function(src) {
      if(!src) return;
      // Ensure the 'src' has no '/' (it's in the root already)
      if(src[0] === '/') src = src.substr(1);
      src = manifest.root + src ;
      // Load javascript
      if(src.substr(-3) === ".js"){
        el= document.createElement('script');
        el.charset="UTF-8";
        el.type= 'text/javascript';
        el.src= src + '?' + now;
        el.async = false;
      // Load CSS
      } else {
        el= document.createElement('link');
        el.rel = "stylesheet";
        el.href = src + '?' + now;
        el.type = "text/css";
      }
      head.appendChild(el);
    });
  }

  //---------------------------------------------------
  // Step 3: Ensure the 'root' end with a '/'
  manifest.root = manifest.root || './';
  if(manifest.root.length > 0 && manifest.root[manifest.root.length-1] !== '/')
    manifest.root += '/';

  // Step 4: Save manifest for next time
  if(!fromLocalStorage) 
    localStorage.setItem('manifest',JSON.stringify(manifest));

  // Step 5: Load Scripts
  // If we're loading Cordova files, make sure Cordova is ready first!
  if(typeof window.cordova !== 'undefined'){
    document.addEventListener("deviceready", loadScripts, false);
  } else {
    loadScripts();
  }
  // Save to global scope
  window.Manifest = manifest;
}
//---------------------------------------------------------------------
``` 
第三段为根据localstorage或者本地manifest.json开始加载

```js
window.Manifest = {};
// Step 1: Load manifest from localStorage
var manifest = JSON.parse(localStorage.getItem('manifest'));
// grab manifest.json location from <script manifest="..."></script>
var s = document.querySelector('script[manifest]');
// Not in localStorage? Fetch it!
if(!manifest){
  var url = (s? s.getAttribute('manifest'): null) || 'manifest.json';
  // get manifest.json, then loadManifest.
  pegasus(url).then(loadManifest,function(xhr){
    console.error('Could not download '+url+': '+xhr.status);
  });
// Manifest was in localStorage. Load it immediatly.
} else {
  loadManifest(manifest,true,s.getAttribute('timeout') || 10000);
}
``` 
3.autoupdate.js解析 

autoupdate.js里定义服务端地址 

初始化CordovaPromiseFS及CordovaAppLoader，并进行比较

4.CordovaPromiseFS.js及CordovaAppLoader.js解析

定义了下载、缓存、文件比较等方法