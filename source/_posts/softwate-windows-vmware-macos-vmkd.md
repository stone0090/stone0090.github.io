title: 收缩VMware虚拟机中Mac系统的vmdk文件
date: 2014-11-16 23:17:00
comments: true
categories: [软件,VMware]
tags: [VMware,Mac,虚拟机]
---

博主标准穷屌丝一枚，虽然一直眼馋mbp，但苦于手头米有限，只是在虚拟机中玩玩MacOS。（我才不会告诉你我笔记本i5+120SSD+500HHD+12G内存，跑MacOS虚拟机一点不卡，貌似还是摆脱不了我是穷屌丝的事实，哎）

今天把虚拟机中的Mavericks升级到Yosemite，准备看看iOS8的新特性，开发个通知中心的app玩玩，谁知道虚拟机vmdk的文件从20G飙升到了38G，120G的ssd果断爆了，虽然系统是升级完成了，但是xcode却没有空间安装了。

<!-- more --> 

经过一番度娘，大家都说升级完Yosemite之后硬盘空间反而会变小，不会变大，我这种情况应该是VMware的增长机制造成的。

只好继续度娘，但是大家的攻略都只是讲的VMware中Windows收缩办法，木有MacOS的。只好墙过之后找谷哥，还是墙外给力，果然找到了一些有用的攻略，被奇奇怪怪的方法坑过n次之后，终于发现了一条非常有用的命令了，在此无私的奉献给大家~

```
// 1. 打开你虚拟机中的Terminal，输入：
sudo  /Library/Application\ Support/VMware\ Tools/vmware-tools-cli disk shrink  / 

// 2. 输入你电脑的密码，等待完成即可。
```

注意：虚拟机所在磁盘最好预留20G以上的可用空间，我就是有好几次因为磁盘空间不足，导致收缩失败

![](http://qn.shisb.com/blog/softwate-windows-vmware-macos-vmkd/1.png)

至于效果嘛，无图无真相，看图吧！！！

![](http://qn.shisb.com/blog/softwate-windows-vmware-macos-vmkd/2.png)

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/41181417)
</div>