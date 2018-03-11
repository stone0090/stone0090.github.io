title: C#技术分享-DOC转图片的3种方法
date: 2012-02-22 19:24:00
comments: true
categories: [技术,.NET]
tags: [DOC转图片,C#,.NET,开源项目]
---

DOC转图片的2种方法，如果看完全文还是不能解决您的问题，请在评论区留言。
- [Demo 百度网盘 下载地址](http://pan.baidu.com/s/1i36HHqP)
- [GitHub Clone 地址](https://github.com/stone0090/OfficeTools.Pdf2Image.Word2Image.git)

## 1.Aspose.Words.dll
- 第三方DLL，可以实现DOC转PDF，DOC转图片。
- 官方试用版有红色水印，博主提供的是没有水印的破解版，但还是希望大家支持正版。

## 2.Microsoft Office2007 ~ Office2015
- Microsoft官方接口，可以实现DOC转PDF，再结合博主的上一篇文章[C#技术分享-PDF转图片的10种方法](/2012/02/15/office-pdf2image/)，即可实现DOC转图片。
- 该方法需要必须先安装Office2007或以上版本（备注：Office2007还需要安装官方插件，已包含在Demo中）。
- 注意：使用该方法，如果Word文档中有表格的话，转成PDF时表格就会不整齐了，使用第三方DLL就不会有这种情况。

<!-- more --> 

## 3.Adobe Acrobat X Pro 的PDF打印机
- Acrobat官方接口，可以实现DOC转PDF，再结合博主的上一篇文章[C#技术分享-PDF转图片的10种方法](/2012/02/15/office-pdf2image/)，即可实现DOC转图片。
- 该方法需要必须先安装Adobe Acrobat X Pro，利用Acrobat PDF打印机生成PDF。
- 注意：使用该方法，有可能出现如下图错误提示。网友支招：“在控制面板中对Adobe PDF打印机的打印属性->高级->打印默认值中的“仅依靠....”勾掉后，可以成功”，具体效果博主并没有亲测，还祝大家好运。
- 由于该方案存在不确定性，所以需要[单独下载](http://pan.baidu.com/s/1ntIhgrJ)。

![](http://qn.shisb.com/blog/office-doc2image/1.gif)

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/7284428)
</div>