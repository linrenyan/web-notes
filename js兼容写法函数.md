
##### 兼容所有浏览器的求滚动条距离
```
function getScrollOffset(){
    if(window.pageXOffset){
        return {
            x : window.pageXOffset,
            y : window.pageYOffset
        }
    }else{
        // 兼容IE8及以下的写法
        return {
            x : document.body.scrollLeft + document.documentElement.scrollLeft,
            y : document.body.scrollTop + document.documentElement.scrollTop
        }
    }
}
```

##### 兼容所有浏览器的求视口宽高
```
// 网页顶部有<!DOCTYPE html>为标准模式，否则为怪异模式
// window.innerWidth/innerHeight    IE8及以下不兼容
// document.documentElement.clientWidth/clientHeight    标准模式下，任意浏览器都兼容
// document.body.clientWidth/clientHeight  适用于怪异模式下的浏览器
function getViewportOffset(){
    if(window.innerWidth){
        return {
            w : window.innerWidth,
            h : window.innerHeight
        }
    }else{
        if(document.compatMode === "BackCompat"){//判断是否为怪异模式
            return {
                w : document.body.clientWidth,
                h : document.body.clientHeight
            }
        }else{
            return {
                w : document.documentElement.clientWidth,
                h : document.documentElement.clientHeight
            }
        }
    }
}
```
#### dom不能直接操作css，但能间接操作css
##### 封装兼容性方法获得dom的样式  getStyle(div,width)

```
function getStyle(elem,prop){
    if(window.getComputedStyle){
        return window.getComputedStyle(elem,null)[prop];
    }else{
        return elem.currentStyle[prop];
    }
}
```
##### 兼容的绑定事件

```
function addEvent(elem,type,handle){
    if(elem.addEventListener){
        elem.addEventListener(type,handle,false);
    }else if(elem.attachEvent){
        elem.attachEvent('on' + type,function(){    //只兼容ie
            handle.call(elem);  //this指向window，所以用call，使this指向elem
        })
    }
}
```
##### 封装取消冒泡的函数

```
function stopBubble(event){
    if(event.stopPropagation){
        event.stopProppagation();
    }else{
        event.cancelBubble = true;
    }
}
```
##### 封装阻止默认事件的函数

```
function cancelHandler(event){
    if(event.preventDefault){
        event.preventDefault();
    }else{
        event.returnValue = false;
    }
}
```
##### 事件对象
```
function test(e){
    var event = e || window.event;
    var target = event.target || event.srcElement;
}
```
##### 异步加载js，按需加载，封装成函数
```
function loadScript(url,callback){
    var script = document.createElement('script');
    script.type = "text/javascript";
    
    script.readyState = "loading";
    if(script.readyState){
        script.onreadystatechange = function(){
            if(script.readyState == "complete" || script.readyState == "loaded"){
                callback();    //兼容id
            }
        }
    }else{
        script.onload = function(){
            callback();//不兼容ie
        }
    }
    script.src = url;   //需放在这里，也就是绑定之后
    document.head.appendChild(script)
}
loadScript("demo.js",function(){});
```
##### css 兼容
```
display:inline-block;   //>=IE8
max-width/min-width  //>=IE7
:before/:after  //>=IE8
div:hover   //>=IE7
background-size  //>=IE9
border-radius   //>=IE9
阴影    //>=IE9
动画/渐变   //>=IE10
*zoom

浏览器兼容库
html5shiv
respond.js
css Reset

caniuse.com //查css属性兼容的
browserhacks    //查css hack
```






