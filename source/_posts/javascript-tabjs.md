title: tab.js分享及浏览器兼容性问题汇总
date: 2016-05-31 23:55:00
comments: true
categories: [技术,JavaScript]
tags: [JavaScript,tab.js] 
---

在 [样式布局分享-基于frozen.js的移动OA](http://shijiajie.com/2016/03/13/frontend-mobileoa-demo/) 文章中，用了到第三方组件 tab.js(带菜单的横屏滑动插件)，其兼容性很差，进行优化后，已兼容全平台（且支持IE6+）。

![](http://qn.shisb.com/blog/javascript-tabjs/1.jpg)

- [tab.js GitHub Clone 地址](https://github.com/stone0090/s-fontend/tree/master/me/tab) 

一直听说过IE6~IE9浏览器的兼容性问题是深坑，这次终于有所体会，就本次优化tab.js而言，如果不对IE6~IE9进行兼容，工作量可以减少一倍。

特此把遇到的各种浏览器兼容性问题进行汇总，希望对大家有所帮助。

<!-- more --> 

### trim（不支持IE6~IE9）
> 说明：去掉字符串中的空格。
``` javascript
// 以下为兼容写法
String.prototype.trim = function () {
    return this.replace(/^\s\s*/, '').replace(/\s\s*$/, '');
}
```

### requestAnimationFrame（不支持IE6~IE9）
> 说明：它是由浏览器专门为动画提供的API，效果和setTimeout/setInterval类似。
``` javascript
// 以下为兼容写法
var rAF = window.requestAnimationFrame ||
    window.webkitRequestAnimationFrame ||
    window.mozRequestAnimationFrame ||
    window.oRequestAnimationFrame ||
    window.msRequestAnimationFrame ||
    function (callback) { window.setTimeout(callback, 1000 / 60); };
```

### addEventListener （不支持IE）
> 说明：为元素绑定事件。
``` javascript
// 以下写法可以兼容大部分情况
var addHandler = function(el, type, handler, args) {
    if (el.addEventListener) {
        el.addEventListener(type, handler, false);
    } else if (el.attachEvent) {
        el.attachEvent('on' + type, handler);
    } else {
        el['on' + type] = handler;
    }
};
var removeHandler = function(el, type, handler, args) {
    if (el.removeEventListener) {
        el.removeEventListener(type, handler, false);
    } else if (el.detachEvent) {
        el.detachEvent('on' + type, handler);
    } else {
        el['on' + type] = null;
    }
};
```

### event.target （不支持IE6~IE9）
> 说明：引发事件的DOM元素。
``` javascript
// 以下为兼容写法
target = event.target || event.srcElement;
```

### event.preventDefault （不支持IE6~IE9）
> 说明：如果事件对象的cancelable属性为true，则该方法可以取消事件的默认动作，但并不取消事件的冒泡行为。（以下为兼容方法）
``` javascript  
// 以下为兼容写法
event.preventDefault ? event.preventDefault() : (event.returnValue = false);
```

### event.stopPropagation（不支持IE6~IE9）
> 说明：阻止事件的冒泡行为。
``` javascript
// 以下为兼容写法
event.stopPropagation ? event.stopPropagation() : (event.cancelBubble = false);
```

### event.touches.pageX（不支持IE6~IE9）
> 说明：鼠标在页面上的位置,从页面左上角开始,即是以页面为参考点,不随滑动条移动而变化。
``` javascript
// 以下为兼容写法
var touches = e.touches ? e.touches[0] : e;
var pageX = (touches.pageX) ? touches.pageX : e.clientX + (document.documentElement.scrollLeft ? document.documentElement.scrollLeft : document.body.scrollLeft);
var pageY = (touches.pageY) ? touches.pageY : e.clientY + (document.documentElement.scrollTop ? document.documentElement.scrollTop : document.body.scrollTop);
```

欢迎关注微信公众号「劼哥舍」，老斯基带你飙车。