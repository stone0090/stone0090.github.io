title: C#技术分享-PDF转图片的10种方法
date: 2012-02-15 18:41:00
comments: true
categories: [技术,.NET]
tags: [PDF转图片,C#,.NET,开源项目] 
---

PDF转图片的10种方法，为了节省大家时间，博主把最常用的方法写在最前面，如果看完全文还是不能解决您的问题，请在评论区留言，或加入QQ群(274281457)进行学习交流。

- [Demo 百度网盘 下载地址](http://pan.baidu.com/s/1eQ6hK90)
- [GitHub Clone 地址](https://github.com/stone0090/OfficeTools.Pdf2Image.Word2Image.git) 

## 1. O2S.Components.PDFRender4NET.dll
- 第三方DLL，可以实现PDF转图片，支持32位系统、64位系统
- 官方试用版有红色水印，博主提供的是没有水印的破解版，但还是希望大家支持正版。
- 博主自己使用的是正版DLL (版本很旧)，稳定运行了好几年，基本没有问题，强烈推荐该方法。
- 福利：在博主微信公众号「劼哥舍」中回复关键字「PDF」，即可领取博主自用正版DLL。

## 2. Acrobat.dll
- Adobe官方接口，可以实现PDF转图片。该方法需要必须先安装Adobe Acrobat X Pro，再从安装目录下找到 **Acrobat.dll** 引用到项目中。
- Acrobat.dll 的转换效率要比其他第三方DLL 要快很多，而且更稳定，但是不支持多线程，所以在iis下会调用失败，有网友先用windows服务来调用Acrobat.dll，再用iis调用windows服务来解决该问题。
- 如果您对转换速度、图片质量要求很高，该方法是最佳选择，但是实现过程最为麻烦。
- 参考资料：http://www.codeproject.com/Articles/5887/Generate-Thumbnail-Images-from-PDF-Documents

<!-- more --> 

## 3. PDFLibNet.dll
- 第三方DLL，可以实现PDF转图片，博主提供的是没有水印的破解版，只支持32位系统。

## 4. SautinSoft.PdfFocus.dll
- 第三方DLL，可以实现PDF转图片，转出来的图片有红色水印。

## 5. TallComponents.PDF.Rasterizer.dll
- 第三方DLL，可以实现PDF转图片，转出来的图片有红色水印。

## 6. Apitron.PDF.Rasterizer.dll
- 第三方DLL，可以实现PDF转图片，转出来的图片有红色水印。

## 7. abcpdf.dll
- 第三方DLL，可以实现PDF转图片，但是需要安装abcpdf，使用起来不太放方便。

## 8. Ghostscript
- 第三方DLL，可以实现PDF转图片，只支持32位系统，网上很多人都采用这个方法（貌似功能很强大），博主觉得代码复杂，没有深入研究。
- 参考资料：http://www.codeproject.com/Articles/317700/Convert-a-PDF-into-a-series-of-images-using-Csharp.aspx
- 参考资料：http://www.codeproject.com/Articles/32274/How-To-Convert-PDF-to-Image-Using-Ghostscript-API

## 9. XpdfRasterizer.dll 
- 第三方DLL，可以实现PDF转图片，不清楚是否有水印。
- DLL 下载地址：http://download.csdn.net/detail/shi0090/4066115 
- 备注：Demo 意外丢失，使用该方法一定要用 **regsvr32** 命令对DLL进行注册，不然会转换失败。

## 10. ImageMagick
- C语言开源工具，可以实现PDF转图片，博主使用的是C#，所以对该方法没有深入研究。
- Demo 下载地址：http://download.csdn.net/detail/shi0090/4066040 

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/7262199)
</div>