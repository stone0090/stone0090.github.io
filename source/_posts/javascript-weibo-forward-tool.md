title: 被「李笑来老师」拉黑之「JavaScript微博自动转发的脚本」
date: 2016-10-02 12:00:00
comments: true
categories: [技术,JavaScript]
tags: [JavaScript] 
---

故事的背景如下图，[李笑来](http://weibo.com/bylixiaolai) 老师于10月19日在 [知乎Live](https://www.zhihu.com/lives/about) 开设 [一小时建立终生受用的阅读操作系统](https://www.zhihu.com/lives/763851343583547392) 的讲座，他老人家看到大家伙报名踊跃，便在微博上发起了一个 [猜数量赢取iPhone7](http://weibo.com/1576218000/Eau3ZoPWd?type=comment) 的活动。

![](http://qn.shisb.com/blog/javascript-weibo-forward-tool/1.jpg)

因为该活动注明了「不限猜的次数」，我便用 JavaScript 写一个自动转发的脚本，用机器代替手工转发，结果转发不到200次就被 [李笑来](http://weibo.com/bylixiaolai) 老师拉黑了，实在扫兴。与其独自郁闷，不如把技术细节分享给大家，祝大家能早日赢得 iPhone7。我的微博地址是：[http://weibo.com/stone0090](http://weibo.com/stone0090)，欢迎大家来围观。

<!-- more --> 

本以为花一两个小时就能搞定这个微博自动转发的脚本，结果中途不停的踩坑折腾了大半天。还好早早的被 [李笑来](http://weibo.com/bylixiaolai) 老师拉黑。不然用 .NET 重写工具，再接入 [打码兔](http://www.dama2.com)，还得再花我好几个小时。好不容易国庆长假休息一下，还不是想给媳妇换个 iPhone7，我就能用她的 iPhone6s，要不然真心不想花太多时间捣鼓这个。废话不多说了，进入正题：

###  前期准备

- JavaScript：如果不会 JavaScript，建议先学完 [JavaScript 闯关记](https://github.com/stone0090/javascript-lessons)，再继续看本文。
- Chrome：开发调试 JavaScript 必备神器。
- 微博会员：据网上流言，普通用户如果转发过多会被封号，而会员则不会。

### 填坑过程

打开 Chrome 浏览器中，先登录自己的微博，再进入李笑来老师的微博首页 http://weibo.com/bylixiaolai 。

![](http://qn.shisb.com/blog/javascript-weibo-forward-tool/3.jpg)

打开 Chrome 开发者工具（Mac 快捷键 `option` + `comand` + `j`，Window 快捷键 `ctrl` + `shift` + `i`），切换 tab 到 NetWork，并点击 clear，清除初始化时所加载的数据。

![](http://qn.shisb.com/blog/javascript-weibo-forward-tool/2.jpg)

然后手动转发一次微博，获取到转发时所产生的请求。

![](http://qn.shisb.com/blog/javascript-weibo-forward-tool/4.jpg)

利用上图红框中的关键数据，使用 JavaScript 模拟发送转发请求，具体代码如下。

```javascript
// 转发微博，并评论
function forwardWeibo(content, retcode) {
  var formData = new FormData();
  formData.append('pic_src', '');
  formData.append('pic_id', '');
  formData.append('appkey', '');
  formData.append('mid', '4024988475919525');
  formData.append('style_type', '1');
  formData.append('mark', '');
  formData.append('reason', content);
  formData.append('location', 'page_100505_home');
  formData.append('pdetail', '1005051576218000');
  formData.append('module', '');
  formData.append('page_module_id', '');
  formData.append('refer_sort', '');
  formData.append('is_comment_base', '1');
  formData.append('rank', '0');
  formData.append('rankid', '');
  formData.append('_t', '0');
  formData.append('retcode', retcode || '');

  var xhr = new XMLHttpRequest();
  xhr.timeout = 3000;
  xhr.responseType = "text";
  xhr.open('POST', 'http://weibo.com/aj/v6/mblog/forward?ajwvr=6&domain=100505&__rnd=' + new Date().getTime(), true);
  xhr.onload = function(e) {
    if (this.status == 200 || this.status == 304) {
      var data = JSON.parse(this.responseText);
      if (data.code == "100000") {
        // 转发微博成功
        console.log(content);
      } else if (data.code == "100027") {
        // 转发微博失败，需要回答图片验证码的问题
        console.log(data);
      } else {
        // 转发微博失败，其他原因
        console.log(data);
      }
    }
  };
  xhr.send(formData);
}
//forwardWeibo('转发内容');
//forwardWeibo('转发内容',verified('答案'));

// 每5秒转发一次
var count = 35000;
setInterval(function(){
  forwardWeibo(count++);
}, 5000); 
```

打开 Chrome 开发者工具，切换 tab 到 Console，拷贝上面代码到 Console 中，按回车键即可以「5秒1次」的频率对李笑来老师的这条微博进行转发评论，如需停止请关闭该页面再重新打开。

然而仅过了2分钟，成功转发50多次之后，后面的转发全部失败。经检查发现，由于我转发频率过快，被微博官方暂时封号。回答一些简单的问题把账号解封，我把转发频率由「5秒1次」改为「10秒1次」，因为除我之外还有其他几个号也在用脚本刷，他们大概用「10秒1次」的频率，稳定的转发没有间断过，所以「10秒1次」应该是相对安全的。

我调整频率之后重新开始转发，但还是转发失败，手动操作后发现转发需要输入验证码，以前并没有这个环节，看来刚才的封号是有一些后遗症的。验证码我才不怕，专业的打码服务 [打码兔](http://www.dama2.com) 连12306的验证码都能轻松应付，识别这里的验证码就是小儿科。但接入 [打码兔](http://www.dama2.com) 的工作量有点大，我还是先找找看，有没有更简单的方法。

果然还真被我找到了，虽然转发的时候需要输入验证码，但评论的时候并不用，手动操作一把，评论并转发也能成功，便马上新增了一个评论的方法，具体代码如下。

```javascript
// 评论微博，并转发
function commentWeibo(content) {
  var formData = new FormData();
  formData.append('act', 'post');
  formData.append('mid', '4024988475919525');
  formData.append('uid', '1760390531');
  formData.append('forward', '1');
  formData.append('isroot', '0');
  formData.append('content', content);
  formData.append('location', 'page_100505_home');
  formData.append('module', 'scommlist');
  formData.append('group_source', '');
  formData.append('tranandcomm', '1');
  formData.append('pdetail', '1005051576218000');
  formData.append('_t', '0');

  var xhr = new XMLHttpRequest();
  xhr.timeout = 3000;
  xhr.responseType = "text";
  xhr.open('POST', 'http://weibo.com/aj/v6/comment/add?ajwvr=6&__rnd=' + new Date().getTime(), true);
  xhr.onload = function(e) {
    if (this.status == 200 || this.status == 304) {
      if (this.responseText.code == "100000") {
        console.log(content);
      } else {
        console.log(this.responseText)
      }
    }
  };
  xhr.send(formData);
}
//commentWeibo('评论内容');

// 每10秒评论一次
var count = 35000;
setInterval(function(){
  forwardWeibo(count++);
}, 10000); 
```

没高兴几分钟，又发现新的问题，评论成功10条，只有1条转发成功了，这完全是坑爹啊。看来只有接入 [打码兔](http://www.dama2.com)  才能彻底解决问题了，估计要花2、3个小时才能搞定，算了，先吃饭、洗澡再弄。

磨蹭了1、2个小时之后回来，发现微博转发输入验证码的限制已经被取消，但我仍心有余悸，把脚本的频率改为「30秒1次」让它慢慢的跑。然后，埋头研究  [打码兔](http://www.dama2.com) 的 API，注册相关开发者账号，充值测试费用。就在我刚准备写代码之际，脚本又失败了，而且，这次的报错跟以前都不一样，原来是我已经被 **李笑来老师拉黑了**，再也不能转发评论他老人家任何微博了。

本以为会刷几万条微博出来，没想到只刷了200条不到，这些微博就留作纪念不删了。下面是提前准备好的批量删微博的脚本。

```javascript
//删除微博
function deleteWeibo() {
  var items = document.querySelectorAll(".WB_feed_type");
  for(var i in items){
    if(items[i].getAttribute){
      var formData = new FormData();
      formData.append('mid', items[i].getAttribute("mid"));
      var xhr = new XMLHttpRequest();
      xhr.open('POST', 'http://weibo.com/aj/mblog/del?ajwvr=6', false);
      xhr.send(formData);
      console.log(xhr.responseText);
    }
  }
}
deleteWeibo();
```

信念瞬间崩塌，思想得到解放，果断去抱着媳妇追 [权利的游戏](https://movie.douban.com/subject/3016187/)，啪啪啪，真是一个美好夜晚。

最后，祝大家国庆节快乐。如果还想听我聊技术（che dan），请关注微信公众号「劼哥舍」，老斯基带你飙车。