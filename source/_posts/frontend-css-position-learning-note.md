title: CSS基础知识之position
date: 2016-04-17 23:05:00
comments: true
categories: [技术,CSS]
tags: [CSS,position] 
---
最近在慕课网学习了 [网页布局基础](http://www.imooc.com/learn/95) 和 [固定层效果](http://www.imooc.com/learn/55) ，都是由声音甜美的 [婧享人生](http://www.imooc.com/u/104592/courses?sort=publish) 老师所录制，视频详细讲解了CSS中position的用法，在此把学习笔记分享给大家。

## CSS定位机制
- 标准文档流（Normal flow）
- 浮动定位（Floats）
- 绝对定位（Absolute positioning）

<!-- more -->

### 标准文档流
- 从上到下，从左到右，输出文档内容
- 由块级元素和行级元素组成

#### 块级元素
- 从左到右撑满页面，独占一行
- 触碰到页面边缘时，会自动换行
- 常见的标签有：div、ul、li、di、dt、p

#### 行级元素
- 能在同一行内显示
- 不会改变HTML文档结构
- 常见的标签有：input、span、label、strong、img

#### 盒子模型
- 边框（border）
- 外边距（margin）
- 内边距（padding）
- 盒子中的内容（content）
- 盒子模型尺寸=边框+外边距+内边距+盒子中内容的尺寸

#### 盒子3D模型
- 第一层：border
- 第二层：content + padding
- 第三层：background-image
- 第四层：background-color
- 第五层：margin

### 浮动定位
- 三个属性：left（左浮动）、right（右浮动）、none（不浮动）
- 元素会左移、或右移、直到碰到容器为止
- 虽然脱离了标准文档流，但情况有些特殊，详情见[CSS基础知识之float](http://shijiajie.com/2016/06/10/frontend-css-float-learning-note)

#### 清除浮动的常用方法
- clear属性，常用clear:both;（当父包含块缩成一条时无效）
- 同时设置width:100%（或固定宽度）+overflow:hidden;

### 相对定位
- 相对于自身原有位置进行偏移
- 仍处于标准文档流中
- 随即拥有偏移属性和z-index属性

### 绝对定位
- 建立了以包含块为基准的定位
- 完全脱离了标准文档流
- 随即拥有偏移属性和z-index属性

#### 绝对定位-偏移参考基准
- 未设置偏移量时，无论是否存在已定位的祖先元素，都保持在元素初始位置
- 已设置偏移量时，无已定位祖先元素，以<html>为偏移参考基准
- 已设置偏移量时，有已定位祖先元素，以距其最近的已定位祖先元素为偏移参考基准

#### 绝对定位-常见问题
- 没有设置宽度时，元素的宽度根据内容进行调节
- 左右布局时，柱子层的高度，一定要大于绝对定位元素的高度

### 固定定位（也属于绝对定位）
- 位置固定，不会随滚动条变化
- 被他遮盖的元素，可以从其下层穿过

#### 固定定位-偏移参考基准
- 未设置偏移量时，无已定位祖先元素，以浏览器窗口为基准定位
- 未设置偏移量时，有已定位祖先元素，以祖先元素为基准定位
- 已设置偏移量时，无论是否存在已定位的祖先元素，均以浏览器窗口为基准定位

欢迎大家来 [石佳劼的博客](http://shijiajie.com) 和我交流，在 [这里](http://shijiajie.com/2016/04/17/frontend-css-position-learning-note/#ds-thread)  留下您宝贵建议。