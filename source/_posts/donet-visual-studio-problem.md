title: 解决Visual Studio“加载...符号缓慢”的问题
date: 2014-02-18 15:34:00
comments: true
categories: [技术,.NET]
tags: [.NET,Visual Studio] 
---

最近在用 Visual Studio 调试时，经常出现“加载....符号缓慢的问题”，快的时候5分钟能出来，慢的时候就直接死掉了。

我一直以为是解决方案中的项目太多导致的（100多个项目），嫖过度娘才发现这是个普遍存在的问题，而且 VS2005、VS2008、VS2010、VS2012、VS2013 均有可能出现这个问题，原因是加载符号是需要联网下载，所以耗费了大量的时间。

**具体解决方法如下：**
1.打开VS的 **工具**->**选项**->**调试**->**符号**
2.取消勾选“Microsoft符号服务器”
3.点击“清空符号缓存”按钮
4.重启 Visual Studio

![](http://qn.shisb.com/blog/donet-visual-studio-problem/1.jpg)

完成以上三步即可解决该问题，大伙可以参考一下。

<!-- more --> 

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/19411777)
</div>