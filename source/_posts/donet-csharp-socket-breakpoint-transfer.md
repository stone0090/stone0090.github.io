title: C#技术分享-基于Socket的断点续传功能
date: 2015-07-12 23:55:00
comments: true
categories: [技术,.NET]
tags: [.NET,C#,Socket,断点续传]
---

最近开发了一个异地灾备传输工具，每周从广州服务器传输 **5G的数据库备份文件** 到北京服务器。

第一个版本的传输工具开发得很简单，两个window服务，一个负责发送，另一个负责接收，利用 socket 建立 tcp 连接进行数据传输，在测试服务器上传输10G大小的文件没有问题。

但部署到生产环境之后，每次传输了3G左右的数据就中断了，经分析是由于网络不稳定造成的，所以需要对传输工具添加断点续传功能，当传输意外中断时，可以自动连接，并完成上一次未完成的传输。

断点续传的原理很简单，就是分割需要传输的文件，每次传输一小块数据，并附带数据的位置和大小信息，服务器成功接收数据之后，则继续下一块数据的传输，否则重复上一块数据的传输，直到成功为止。

<!-- more --> 

这实际上是为传输功能添加了暂停功能，网络中断的时候暂停传输了，网络恢复之后继续再传。

既然搞清楚原理了我们就赶紧开始写代码吧，下面是核心代码：

---
封装一个类包含一下字段，用来记录传输的状态：FileName 文件名、FileSize 文件大小、PackaGeSize 数据包的大小、PackaGeCount 传输总次数，Index 当前传输位置。每次发送数据包时，带上这些信息，就算意外中断了，重新连上了之后，也可以很轻易的判断传输的进度。
```
public class BreakPointPost
{
    public string FileName { get; set; }
    public long FileSize { get; set; }
    public long PackageSize { get; set; }
    public int PackageCount { get; set; }
    public int Index { get; set; }
}
```

---
获取文件的传输次数
```
private static int GetFilePackageCount(long fileSize, long packageSize)
{
    int count = 0;
    if (fileSize % packageSize > 0)
        count = Convert.ToInt32(fileSize / packageSize) + 1;
    else
        count = Convert.ToInt32(fileSize / packageSize);
    return count;
}
```

---
分块读取文件
```
private static byte[] FileRead(string path, int index, long size)
{
    byte[] result = null;
    long length = (long)index * (long)size + size;
    using (FileStream stream = File.OpenRead(path))
    {
        if (length > stream.Length)
            result = new byte[stream.Length - ((long)index * (long)size)];
        else
            result = new byte[size];
        stream.Seek((long)index * (long)size, SeekOrigin.Begin);
        stream.Read(result, 0, result.Length);
    }
    return result;
}
```

---
分块接受文件
```
private static void FileWrite(string path, int index, long packageSize, int receiveSize, byte[] data)
{
    using (System.IO.FileStream stream = System.IO.File.OpenWrite(path))
    {
        stream.Seek((long)index * (long)packageSize, System.IO.SeekOrigin.Begin);
        stream.Write(data, 0, receiveSize);
        stream.Flush();
    }
}
```
- [控制台版 Demo 下载地址（测试版）](http://yun.baidu.com/s/1v1j9G)
- [Windows 服务版 Demo 下载地址（生产版）](http://yun.baidu.com/s/1c00gKIs)
- [GitHub Clone 地址](https://github.com/stone0090/library-code/tree/master/donet/socket)

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/46855097)
</div>