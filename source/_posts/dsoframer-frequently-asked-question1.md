title: DSOframer无法正常加载的解决方案
date: 2015-08-31 12:00:00
comments: true
categories: [技术,DSOframer]
tags: [DSOframer]
---

不了解 DSOframer 的朋友，可以先参考文章 [DSOframer 的简单介绍和资源整理](/2015/09/02/dsoframer-introduction-resources/)。

在使用 DSOframer 时，经常会碰到各种奇奇怪怪的问题，最常见的就是控件不能正常加载。大家可以按下面方法进行排查，如果还是不能正常加载，那就只有重装office或操作系统了。

### 1.重新注册 DSOframer 控件
- 解压 `DSOframer.CAB`，拷贝 `DSOframer.ocx` 控件到 C:\Windows\System32 下，用管理员身份打开同目录下的 `cmd.exe`，执行命令 `regsvr32 DSOframer.ocx`，显示 `DllRegisterServer 在 ***.dll 已成功`。
- 解压 `DSOframer.CAB`，拷贝 `DSOframer.ocx` 控件到 C:\Windows\SysWOW64 下，用管理员身份打开同目录下的 `cmd.exe`，执行命令 `regsvr32 DSOframer.ocx`，显示 `DllRegisterServer 在 ***.dll 已成功`。
- 备注：32位系统只需执行第一步，64位系统最好以上两步都执行。

### 2.确保您使用的是IE浏览器（或IE内核的浏览器），并清除缓存、加入可信站点、调整安全级别、调整安全设置、使用兼容性视图。
- 打开IE菜单 `工具->Internet选项`，选择 `常规` 选项卡，点击 `删除` 按钮，在弹出的窗口中点击 `删除` 按钮。
- 打开IE菜单 `工具->Internet选项`，选择 `安全` 选项卡，点击 `站点` 按钮，把您的网站加入到 `可信站点列表` 中。
- 打开IE菜单 `工具->Internet选项`，选择 `安全` 选项卡，点击 `默认级别` 按钮，将 `安全级别` 设置为 `低`。
- 打开IE菜单 `工具->Internet选项`，选择 `安全` 选项卡，点击 `自定义级别` 按钮，将 `对未标记为可安全执行脚本的ActiveX控件初始化并执行脚本` 设置为 `启用`。
- 打开IE菜单 `工具->兼容性视图设置`，点击 `添加` 按钮，把您的网站加入到 `兼容性视图列表` 中。（受影响的IE版本：IE10）

### 3.其他注意事项
- 请尝试使用管理员身份打开IE浏览器。
- 请确保您的 office 是完全安装版，而不是绿色版。
- 64位的操作系统，必须使用32位的IE，才能正常打开 DSOframer 控件。（受影响的系统：Win7、Vista、Server2008、Win8，不受影响的系统：Win8.1、Win10）

以上所有操作均需要完全关闭IE浏览器之后再重试。

<!-- more --> 

