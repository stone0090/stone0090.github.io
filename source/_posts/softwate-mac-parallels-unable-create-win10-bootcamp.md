title: 解决Boot Camp虚拟机升级到Windows 10后Parallels Desktop不能识别的问题
date: 2015-08-01 09:30:00
comments: true
categories: [软件,Parallels]
tags: [Windows 10,Boot Camp,Mac,虚拟机,Parallels]
---

最近几天 Win10 正式版开始推送了，对于喜欢折腾的博主，在第一时间就把 Mac 中 Boot Camp 从 Win7 升级到 Win10，初步体验还不错，等博主用过一段时间之后，再来给大家分享使用心得。

觉得 Win10 开机速度慢的童鞋，可以参考帖子 [Windows 10 第一天使用总结](http://www.cnblogs.com/sunkaixuan/p/4690994.html)，进行优化。

博主日常主要使用 Mac 系统，Windows 系统只是用来写 C# 代码，所以在 Parallels Desktop（以下简称PD10）虚拟机下运行 Boot Camp 就足够了。

可是，升级到 Win10 之后，用 PD10 打开 Boot Camp 虚拟机报错，提示现有的 **虚拟机文件** 和 **Boot Camp 版本** 不匹配，请移除虚拟机再重新导入。

按提示移除了虚拟机，重建时却找不到 “**导入 Boot Camp**” 的选项，百度了整晚，也没找有效的解决方案，最后，终于在 Parallels 官方知识库中发现了类似的问题，特此跟大家分享。

<!-- more --> 

该文章主要解决了 Parallels 无法导入 Boot Camp 基于 Win10 预览版的虚拟机，亲测在 Win10 正式版上有效。

译文如下（博主英语水平有限，翻译的不好请见谅）：

> ##### 问题：不能创建 Boot Camp 基于 Windows 10 预览版的虚拟机，安装界面中没有导入 Boot Camp 的选项。
> ##### 原因：Parallels Desktop 10 只是实验性的支持 Windows 10 预览版系统。
> - **解决方案：**
> - 启动 Boot Camp 中的 Windows 10 系统。
> - 拷贝一份 C:/Windows/inf/volume.inf 文件的副本，并改名为 volume.inf.bak。
> - 下载 [volume.inf](http://pan.baidu.com/s/1jGlHxPk)，替换 C:/Windows/inf/volume.inf 文件。
> - 启动 Mac 系统。
> - 确保已使用 Boot Camp 安装了 Windows 10。
> - 打开 Parallels Desktop。
> - 移除已存在的 Boot Camp 虚拟机。
> - 尝试重建 Boot Camp 虚拟机。
> 
> ##### 备注：您在进行第3步操作的时，可能会存在权限不足，而导致替换 volume.inf 文件失败。
> - **解决方案：**
> - 在 Mac 系统下安装应用 Paragon NTFS for Mac，[官方下载地址](http://dl.paragon-software.com/demo/ntfsmac_trial_u.dmg)，[百度网盘地址](http://pan.baidu.com/s/1c04Q4b2)。 
> - 打开 **系统偏好设置** -> **NTFS for Mac OS X** -> **通用** ，等待 NTFS 磁盘检测，稍后会提示重启电脑。
> - 电脑重启之后，Boot Camp 磁盘中的 NTFS 文件不再是只读的了，然后在 Mac 系统下进行上面第3步替换操作即可。
> 
> 原文链接：[Unable to create Boot Camp based virtual machine with Windows 10 Technical Preview](http://kb.parallels.com/en/122808)

温馨提示：在完成 Boot Camp 导入之后，最好卸载掉 Paragon NTFS for Mac，因为它会导致 PD10 虚拟机不能进行中止操作。