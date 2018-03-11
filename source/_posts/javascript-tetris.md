title: JavaScript版俄罗斯方块
date: 2012-03-24 14:37:00
comments: true
categories: [技术,JavaScript]
tags: [JavaScript] 
---

前段时间逛 CSDN 论坛，看到大牛贴出来的 JavaScript 版俄罗斯方块，实在是佩服之极。博主刚转 Web 开发，正在学习 JavaScript，所以也照着写了个练练手。后来才发现，**看得懂** 和 **写得出来** 真是天壤之别，多次回顾大牛的代码之后，才勉强完成。接下来博主会进一步完善各种功能，争取做一个高品质的俄罗斯方块。[演示地址](/other/javascript-game/tetris/)

## 更新日志
---

### 2012-05-12
- 终于发现问题的原因了，首先感谢“Ericpoon智”，真是没有什么问题可以难到他的。原来在android系统的浏览器google做了特别的限制，button的onclick事件，会延迟300毫秒才会执行，因为怕识别不出来双击操作，如果想迅速的响应按钮，则需要使用ontouchend事件，经过稍微的修改之后，这个俄罗斯方块终于可以在手机上正常游戏了，哈哈，用自己写的游戏来打发时间还是最爽的！

### 2012-03-24
- 加入按钮支持触屏手机操作，但是遇到一个问题，为神马手机上点按钮反应会那么慢呢？？？这样完全不能正常游戏嘛，求高人解释一下~~谢谢啦~

<!-- more --> 

### 2012-03-22
- 暂停功能 
- 瞬落功能 
- 调整预告和计分栏的位置

### 2012-03-20
- 加入方块预告和计分

```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title></title>
<style type="text/css">
.activityModel { margin: 1px; width: 19px; height: 19px; background: red; position: absolute; }
.forecastModel { margin: 0.5px; width: 9.5px; height: 9.5px; background: yellow; position: absolute; }
.stationaryModel { margin: 1px; width: 19px; height: 19px; background: gray; position: absolute; }
.controlModel { margin: 5px; width: 60px; height: 60px; background: #ADD8E6; position: absolute; text-align: center; font-size: 10pt; line-height: 50px; }
.container { top: 0px; left: 0px; background: black; position: absolute; }
.info { top: 0px; position: absolute; font-size: 10pt; word-break: break-all; }
.score { width: 80px; height: 20px; font-size: 10pt; text-align: right; color: white; position: absolute; }
</style>
<script type="text/javascript">
var tetris;
var container;
var intervalId;
var intervalId2;
var speed = 600;
var size = 20;
var forecastSize = 10;
var controlSize = 65;
var rowCount = 18;
var colCount = 10;
var announcement = 6;
var isOver = false;
var isStop = false;
var shapes = ("0,1,1,1,2,1,3,1;1,0,1,1,1,2,2,2;2,0,2,1,2,2,1,2;0,1,1,1,1,2,2,2;1,2,2,2,2,1,3,1;1,1,2,1,1,2,2,2;0,2,1,2,1,1,2,2").split(";");

function createElm(tag, css, id) {
var elm = document.createElement(tag);
elm.className = css;
if (id) elm.id = id;
document.body.appendChild(elm);
return elm;
}

// 检查是电脑登录还是手机登录的
function isFromMobile() {
// var userAgentKeywords=new Array("Android", "iPhone" ,"SymbianOS", "Windows Phone", "iPad", "iPod", "MQQBrowser");
// exclude ms-os and mac-os 
var flag;
var userAgentInfo = navigator.userAgent;
if (userAgentInfo.indexOf("Windows NT") == -1)
flag = true;
else
flag = false;
return flag;
}

function Tetris(css, x, y, shape) {
// 创建4个div用来组合出各种方块
var myCss = css ? css : "activityModel";
this.divs = [createElm("div", myCss), createElm("div", myCss), createElm("div", myCss), createElm("div", myCss)];
if (!shape) {
// 初始化计分栏 
this.score = createElm("div", "score");
this.score.style.top = "0px";
this.score.style.left = 6 * size + "px";
this.score.innerHTML = "score:000";
// 初始化预告栏和预告方块
var forecastCss = "forecastModel";
this.divs2 = [createElm("div", forecastCss), createElm("div", forecastCss), createElm("div", forecastCss), createElm("div", forecastCss)];
}
this.container = null;
this.refresh = function() {
this.x = (typeof (x) != 'undefined') ? x : 3;
this.y = (typeof (y) != 'undefined') ? y : 0;


// 如果有传参，优先使用参数的，如果有预告，优先使用预告，都没有就自己生成
if (shape)
this.shape = shape;
else if (this.shape2)
this.shape = this.shape2;
else
this.shape = shape ? shape : shapes[Math.floor((Math.random() * shapes.length - 0.000000001))].split(",");	
this.shape2 = shapes[Math.floor((Math.random() * shapes.length - 0.000000001))].split(",");	
if (this.container && !this.container.check(this.x, this.y, this.shape)) {
isOver = true;
var c = confirm("重新开始？");
if (c) location.reload();
}
else {
this.show();
this.showScore();
this.showAnnouncement();
}
}

// 显示方块
this.show = function() {
for (var i in this.divs) {
this.divs[i].style.top = (this.shape[i * 2 + 1] - -this.y) * size + "px";
this.divs[i].style.left = (this.shape[i * 2] - -this.x) * size + "px";
}
}

// 显示预告
this.showAnnouncement = function() {
for (var i in this.divs2) {
this.divs2[i].style.top = (this.shape2[i * 2 + 1] - -0.2) * forecastSize + "px";
this.divs2[i].style.left = (this.shape2[i * 2] - -0.2) * forecastSize + "px";
}
}

// 显示分数
this.showScore = function() {
if (this.container && this.score) {
switch ((this.container.score + "").length) {
case 1:
this.score.innerHTML = "score:" + "00" + this.container.score;
break;
case 2:
this.score.innerHTML = "score:" + "0" + this.container.score;
break;
default:
this.score.innerHTML = "score:" + this.container.score;
break;
}
}
}

// 水平移动方块的位置
this.hMove = function(step) {
if (isOver || isStop) return;
if (this.container.check(this.x - -step, this.y, this.shape)) {
this.x += step;
this.show();
}
}

// 垂直移动方块位置
this.vMove = function(step) {
if (isOver || isStop) return;
if (this.container.check(this.x, this.y - -step, this.shape)) {
this.y += step;
this.show();
}
else {
this.container.fixShape(this.x, this.y, this.shape);
this.container.findFull();
this.refresh();
clearInterval(intervalId2);
}
}

this.quickDown = function() {
if (isOver || isStop) return;
intervalId2 = setInterval("tetris.vMove(1)", 1);
}

// 旋转方块
this.rotate = function() {
if (isOver || isStop) return;
var newShape = [this.shape[1], 3 - this.shape[0], this.shape[3], 3 - this.shape[2], this.shape[5], 3 - this.shape[4], this.shape[7], 3 - this.shape[6]];
if (this.container.check(this.x, this.y, newShape)) {
this.shape = newShape;
this.show();
}
}
this.refresh();
}

function Container() {
this.init = function() {

// 绘制方块所在区域
var bgDiv = createElm("div", "container");
bgDiv.style.width = size * colCount + "px";
bgDiv.style.height = size * rowCount + "px";

// 绘制预告所在区域
var anDiv = createElm("div", "info");
anDiv.style.top = forecastSize + "px";
anDiv.style.left = size * colCount - -forecastSize + "px";

if (isFromMobile()) {
// 绘制控制器所在区域
var ctShape = [0, 0, 1, 0, 2.5, 0, 3.5, 0];
var ctDiv = [createElm("div", "controlModel", "leftEvent"),
createElm("div", "controlModel", "rightEvent"),
createElm("div", "controlModel", "rotateEvent"),
createElm("div", "controlModel", "quickdownEvent")]
for (var i in ctDiv) {
ctDiv[i].style.top = (ctShape[i * 2 + 1]) * controlSize - -forecastSize - -rowCount * size + "px";
ctDiv[i].style.left = (ctShape[i * 2]) * controlSize - -forecastSize + "px";
}

// 绑定按钮事件和文字
var leftEvent = document.getElementById("leftEvent");
var rightEvent = document.getElementById("rightEvent");
var quickdownEvent = document.getElementById("quickdownEvent");
var rotateEvent = document.getElementById("rotateEvent");

leftEvent.ontouchend = function() { tetris.hMove(-1); };
rightEvent.ontouchend = function() { tetris.hMove(1); };
quickdownEvent.ontouchend = function() { tetris.quickDown(); };
rotateEvent.ontouchend = function() { tetris.rotate(); };

leftEvent.innerHTML = "左";
rightEvent.innerHTML = "右";
quickdownEvent.innerHTML = "瞬落";
rotateEvent.innerHTML = "旋转";

bgDiv.ontouchend = function() { toggleInterval(); };
anDiv.innerHTML = "玩法说明：<br/>瞬落：'空格'<br/>暂停：'点击黑色'<br/>暂停：'点击方块'<br/>旋转：'上方向键'";
} else {
bgDiv.onclick = function() { toggleInterval(); };
anDiv.innerHTML = "玩法说明：<br/>瞬落：'空格'<br/>暂停：'回车'<br/>暂停：'点击方块'<br/>旋转：'上方向键'";
}

// 加入作者信息
anDiv.innerHTML = anDiv.innerHTML + '<br/><br/>作者：stone<br/>微博：<a href="http://weibo.com/605494869">weibo.com/605494869</a>';
// 清空积分
this.score = 0;
}

this.check = function(x, y, shape) {
// 检查边界越界
var flag = false;
var leftmost = colCount;
var rightmost = 0;
var undermost = 0;
for (var i = 0; i < 8; i += 2) {
// 记录最左边水平坐标
if (shape[i] < leftmost)
leftmost = shape[i];
// 记录最右边水平坐标
if (shape[i] > rightmost)
rightmost = shape[i];
// 记录最下边垂直坐标
if (shape[i + 1] > undermost)
undermost = shape[i + 1];
// 判断是否碰撞
if (this[(shape[i + 1] - -y) * 100 - -(shape[i] - -x)])
flag = true;
}

// 判断是否到达极限高度
for (var m = 0; m < 3; m++)
for (var n = 0; n < colCount; n++)
if (this[m * 100 + n])
flag = true;
if ((rightmost - -x + 1) > colCount || (leftmost - -x) < 0 || (undermost - -y + 1) > rowCount || flag)
return false;
return true;
}

// 用灰色方块替换红色方块，并在容器中记录灰色方块的位置
this.fixShape = function(x, y, shape) {
var t = new Tetris("stationaryModel", x, y, shape);
for (var i = 0; i < 8; i += 2)
this[(shape[i + 1] - -y) * 100 - -(shape[i] - -x)] = t.divs[i / 2];
}

// 遍历整个容器，判断是否可以消除
this.findFull = function() {
var s = 0;
for (var m = 0; m < rowCount; m++) {
var count = 0;
for (var n = 0; n < colCount; n++)
if (this[m * 100 + n])
count++;
if (count == colCount) {
s++;
this.removeLine(m);
}
}
this.score -= -this.calScore(s);
}

this.calScore = function(s) {
if (s != 0)
return s - -this.calScore(s - 1)
else
return 0;
}

// 消除指定一行方块
this.removeLine = function(rowCount) {
// 移除一行方块
for (var n = 0; n < colCount; n++)
document.body.removeChild(this[rowCount * 100 + n]);
// 把所消除行上面所有的方块下移一行
for (var i = rowCount; i > 0; i--) {
for (var j = 0; j < colCount; j++) {
this[i * 100 - -j] = this[(i - 1) * 100 - -j]
if (this[i * 100 - -j])
this[i * 100 - -j].style.top = i * size + "px";
}
}
}
}

function init() {
container = new Container();
container.init();
tetris = new Tetris();
tetris.container = container;
// 绑定键盘事件
document.onkeydown = function(e) {
var e = window.event ? window.event : e;
if (e.keyCode == 13) toggleInterval();
if (isOver || isStop) return;
switch (e.keyCode) {
case 32: // quickDown
tetris.quickDown();
break;
case 38: //rotate
tetris.rotate();
break;
case 40: //down
tetris.vMove(1);
break;
case 37: //left
tetris.hMove(-1);
break;
case 39: //right
tetris.hMove(1);
break;
}
}

// 方块开始下降
intervalId = setInterval("if(!isOver) tetris.vMove(1)", speed);
}

// 控制暂定
function toggleInterval() {
if (isStop) {
isStop = false;
intervalId = setInterval("if(!isOver) tetris.vMove(1)", speed);
}
else {
isStop = true;
clearInterval(intervalId);
}
}
</script>
</head>
<body onload="init()"></body>
</html>
```

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/7390105)
</div>