title: Global.asax类中Application_End方法的触发问题
date: 2012-11-08 07:09:00
comments: true
categories: [技术,.NET]
tags: [ASP.NET,.NET]
---

最近两天在 ASP.NET 项目中碰到一个奇怪的问题，Global.asax 类中 Application_End 方法总是莫名其妙的被触发，导致session丢失。

在网查看了一些资料，主要有以下2种情况触发 Application_End 方法（亲测确实如此）：
1.修改了 web.config
2.修改了 bin 目录下的文件

而我并没有修改 web.config 或 bin 目录下的文件，可还是触发了 Application_End 方法。正当我毫无头绪之时，同事从 StackOverFlow 上找到了一个帖子，帮我解决了这个问题，非常感谢 [Ericpoon智](http://weibo.com/ericpoon2)。

博主对 [原贴](http://stackoverflow.com/questions/2248825/asp-net-restarts-when-a-folder-is-created-renamed-or-deleted) 中的部分内容进行了翻译，如下：

<!-- more --> 

> 1. 新建一个网站项目到 c:\projects\restart-demo
2. 添加默认的 web.config 和一个测试页面 text.aspx
3. 映射IIS站点到目录 c:\projects\restart-demo
4. 使用性能监视器，健康监测监控，跟踪 Global.asax 和 Application_End 
5. 在浏览器中请求页面 http://localhost/test.aspx （Global.asax 中的 Application_Start 被触发）
6. 新建一个文件夹 c:\projects\restart-demo\asdf （Global.asax 中的 Application_End 被触发）
7. 再次在浏览器中请求页面 http://localhost/test.aspx （ Global.asax 中的 Application_Start 被触发）
8. 重命名文件夹 c:\projects\restart-demo\asdf 为 c:\projects\restart-demo\asdf1 （Global.asax 中的 Application_Start 被触发）
9. 测试完成

> 我们使用一个后台内容管理系统去生成“文件”和“文件夹”在 ASP.NET 网站中，用户能 增/删/改/上传 文件到网站上。我们注意到一个问题，当用户 创建、重命名、删除 一个文件夹时，会引起 App Domain 重启了，从而导致 session、cache 全部丢失。请注意，它并不是一个特殊的文件夹，有什么途径来防止这个问题吗？

> 这个问题妨碍网站正常运的原因有2个：
1.当网站重启时，Cache 被销毁了
2.当网站重启时，App domain 需要被重新创建

我根据原帖中的方法进行了测试，结果和原帖稍有区别，只有在删除文件夹时，才会触发 Application_End，其他操作时并无影响，所以博主只要不删文件夹，就可以避免这个问题了。[测试 Demo 下载地址](http://pan.baidu.com/s/1hqD5UcO)  

测试结果如下：

> 2012-11-08 11:16:05.986 Application_BeginRequest
> 2012-11-08 11:16:05.986 Application_AuthenticateRequest
> 2012-11-08 11:16:05.987 Page_Load(第一次加载页面)

> 2012-11-08 11:16:18.739 Application_BeginRequest
> 2012-11-08 11:16:18.739 Application_AuthenticateRequest
> 2012-11-08 11:16:18.740 Page_Load
> 2012-11-08 11:16:18.742 Button1_Click(新增文件)

> 2012-11-08 11:16:23.077 Application_BeginRequest
> 2012-11-08 11:16:23.078 Application_AuthenticateRequest
> 2012-11-08 11:16:23.078 Page_Load
> 2012-11-08 11:16:23.080 Button2_Click(新增文件夹)

> 2012-11-08 11:16:32.542 Application_BeginRequest
> 2012-11-08 11:16:32.543 Application_AuthenticateRequest
> 2012-11-08 11:16:32.544 Page_Load
> 2012-11-08 11:16:32.546 Button3_Click(删除文件)

> 2012-11-08 11:16:39.020 Application_BeginRequest
> 2012-11-08 11:16:39.020 Application_AuthenticateRequest
> 2012-11-08 11:16:39.021 Page_Load
> 2012-11-08 11:16:39.022 Button4_Click(删除文件夹)
> 2012-11-08 11:16:39.025 Session_End
> 2012-11-08 11:16:39.045 Application_End

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/8080598)
</div>