title: DSOframer微软官方API的查阅方法
date: 2015-09-11 12:00:00
comments: true
categories: [技术,DSOframer]
tags: [DSOframer]
---

不了解 DSOframer 的朋友，可以先参考文章 [DSOframer 的简单介绍和资源整理](/2015/09/02/dsoframer-introduction-resources/)。

大家在使用 DSOframer 时，常常会不知道在哪里查 API 文档，网上的文章都非常零散，很难找到自己想要的方法。其实只要你的电脑安装了 Office 软件，就已经有了微软官方的 API 文档，你要做的仅仅是，找到它，然后查阅它。

### 下面以 Office 2007 举例：  
如果您的 Office 是默认安装的，那么 API 文档应该在 `C:\Program Files (x86)\Microsoft Office\Office12\2052` 目录下，找到后缀名为 DEV.HXS 的文件，这些就是你所需要的 API 文档。如下图：

![](http://qn.shisb.com/blog/dsoframer-frequently-asked-question2/1.png)

打开 CMD 命令提示符窗口，输入以下命令即可打开对应的 API 文档了，最好把下面的命令保存成批处理文件，方便下一次使用。


<!-- more --> 

```
"C:\Program Files (x86)\Microsoft Office\Office12\CLVIEW.EXE" "EXCEL.DEV" "office"

"C:\Program Files (x86)\Microsoft Office\Office12\CLVIEW.EXE" "WINWORD.DEV" "office"

"C:\Program Files (x86)\Microsoft Office\Office12\CLVIEW.EXE" "POWERPNT.DEV" "office" 
```

**特别提醒：以上路径仅在没有修改过默认安装路径的 Office 2007 下生效，如果您使用的是其他版本的 Office 或 修改过安装路径，请自行调整上面命令的路径。**

在 Office 帮助窗口中，一定要开启菜单栏中的 `显示目录` 和 右下角的链接状态中的 `仅显示来自此计算机的内容`，不然看不到任何内容。

![](http://qn.shisb.com/blog/dsoframer-frequently-asked-question2/2.png)

官方的 API 文档超级详细，只是示例代码都是 VBScript，还请各位童鞋自行翻译成 JavaScript。



