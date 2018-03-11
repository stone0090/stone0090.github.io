title: 导出Excel失败“服务器无法在发送HTTP标头之后设置内容类型”
date: 2015-03-27 10:06:00
comments: true
categories: [技术,.NET]
tags: [ASP.NET,.NET,Excel] 
---

昨天用户反映导出 excel 失败，我试了一下，只有在导出数据量稍大时，才会出现这个问题。上了半天度娘也没有找到合适的解决方案，突然想到系统其他模块也有导出 excel 功能，便参照相关代码进行了修改，虽然解决了问题，但原理还是不懂，如果哪位朋友知道其原理，还望指导。

<!-- more --> 

具体解决方法如下：

![](http://qn.shisb.com/blog/asp.net-office-export-excel-http-header-error/1.jpg)
![](http://qn.shisb.com/blog/asp.net-office-export-excel-http-header-error/2.jpg)

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/44672077)
</div>