title: CSS基础知识之float
date: 2016-06-10 21:00:00
comments: true
categories: [技术,CSS]
tags: [CSS,float] 
---
前段时间写过一篇[CSS基础知识之position](http://shijiajie.com/2016/04/17/frontend-css-position-learning-note/)，当时对float的理解不太准确，被慕课网多名读者指出（原文已修正，如有误导实在抱歉）。现对float进行更深入的学习，在此把学习心得分享给大家。

### 浮动的基础知识
- 浮动有4个属性：left（左浮动）、right（右浮动）、none（不浮动）、inherit（继承）。
- 浮动元素的包含块是其最近的块级祖先元素。
![](http://qn.shisb.com/blog/frontend-css-float-learning-note/2.jpg)

<!-- more -->

- 浮动元素会左偏移（或右偏移），直到它的外边界接触到『包含块的内边界』或『另一个浮动元素的外边界』。
![](http://qn.shisb.com/blog/frontend-css-float-learning-note/3.jpg)

- 浮动元素脱离了标准文档流，文字和行级元素会环绕该元素，块级元素则不受影响。
![](http://qn.shisb.com/blog/frontend-css-float-learning-note/1.jpg)

- 浮动一个非替换元素，必须为该元素声明一个width，否则元素的宽度趋于0。
![](http://qn.shisb.com/blog/frontend-css-float-learning-note/6.jpg)

- 浮动元素的margin（外边距）不会与其他元素的margin合并。
![](http://qn.shisb.com/blog/frontend-css-float-learning-note/4.jpg)
![](http://qn.shisb.com/blog/frontend-css-float-learning-note/5.jpg)

### 浮动的深入研究
- 浮动元素的顶边不可以高于包含块中先前产生的块级元素或行级元素的顶。
- 浮动元素之间不可重叠，如果水平方向没有足够的空间放置浮动元素，它将向下移动，直到有足够的空间或没有更多的浮动元素为止。
- 浮动元素不能溢出包含块的左、右、上边界，仅可溢出下边界。（浮动元素溢出下边界时，部分浏览器会增加包含块的高度，使浮动元素能够包含在包含块中，出现大片空白，导致浏览器兼容性问题。）
- 浮动元素设置负外边距时，虽然浮动元素看起来溢出了包含块，但实际并没有违反上述规则。
- 特殊情况，浮动元素比包含块更宽时，浮动元素会在偏移的反方向溢出。

### 浮动的负作用
- 背景不能显示
- 边框不能撑开
- margin padding 不能正确显示

### 清除浮动的方法
``` CSS
.clear-float1{ content: “”; display: block; clear: both; }
/* 方法1，当父包含块缩成一条时无效 */
 
.clear-float2{ overflow:hidden; width:100%; }
/* 方法2，overflow:hidden属性相当于是让父级紧贴内容，这样即可紧贴其对象内内容，从而实现了清除浮动。 */
 
.clear-float3{ overflow: auto; zoom: 1; } 
/* 方法3，zoom是在处理兼容性问题，hidden和auto都能清除浮动，据说auto对seo更友好 */
```

### 扩展阅读
- [《CSS权威指南》之浮动和定位](https://book.douban.com/subject/2308234/)
- [CSS基础知识之position](http://shijiajie.com/2016/04/17/frontend-css-position-learning-note)，了解块级元素和行级元素
- [非替换元素和替换元素](http://www.cnblogs.com/wkylin/archive/2011/05/12/2044328.html)
- [KB011: 浮动( Floats )](http://www.w3help.org/zh-cn/kb/011/)
- [KB008: 包含块( Containing block )](http://www.w3help.org/zh-cn/kb/008/)
- [css清除浮动float的三种方法总结](http://my.oschina.net/leipeng/blog/221125)

欢迎大家来 [石佳劼的博客](http://shijiajie.com) 和我交流，在 [这里](http://shijiajie.com/2016/06/10/frontend-css-float-learning-note/#ds-thread)  留下您宝贵建议。
