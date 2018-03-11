title: 《Node.js开发指南》MicroBlog 项目的问题汇总
date: 2014-04-02 19:10:00
comments: true
categories: [技术,JavaScript]
tags: [Node.js,JavaScript,MicroBlog,Node.js开发指南] 
---

最近啃完了《Node.js开发指南》一书，并按书中 Demo 敲完了的所有代码。Node.js 发展的很快，书中使用的组建目前都已经是旧版本，导致 Demo 并不能正常运行，博主记录了开发 MicroBlog 项目时遇到的所有问题，特此分享给大家。

- 问题一：
```
// 安装 ejs 模板的语法有问题，安装不成功，如下：
express -t ejs microblog

// 需要改成：
express microblog -e
```

- 问题二：
```
<!-- partial 方法已经不能用了，可以用include代替，如下： -->
<ul><%- partial('listitem', items) %></ul>

<!-- 需要改成：-->
<% items.forEach(function(listitem){ %>
<% include listitem %>
<% }) %>
```

<!-- more --> 

- 问题三：
```
// helpers 和 dynamicHelpers 方法已经不能用了，如下：
app.helpers({
     inspect: function(obj) {
          return util.inspect(obj, true);
     }
});
app.dynamicHelpers({
     headers: function(req, res) {
          return req.headers;
     }
});
app.get('/helper', function(req, res) {
     res.render('helper', {
          title: 'Helpers'
     });
});

// 需要改成：
var util = require('util');
app.locals({
     inspect: function(obj){
          return util.inspect(obj, true);
     }
});
app.use(function(req, res, next){
     res.locals.headers = req.headers;
     next();
});
app.get('/helper', function(req, res){
     res.render('helper',{
          title: 'Helpers'
     });
});

// 还需要注意的是，上面这段代码需要放在 app.use(app.router); 前面。
```

- 问题四：

> 问题：express3.*已经不支持layout方法了，所以要改成include才能正常显示首页。
> 1.在 views 文件夹下新建，header.ejs 和 footer.ejs。
> 2.layout.ejs 中的内容，以 <%- body %> 为界限，上面的内容写入header.ejs ，下面的内容写入footer.ejs
> 3.然后在 index.js 中加入 <% include header.ejs %> 和 <% include footer.ejs %>，把表单内容，写在中间即可


- 问题五：
```
// 问题：配置mongodb时，很多报错：

// app.js中的 
var settings = require('../settings'); 
// 应改成 
var settings = require('./settings');

// app.js中的 
app.use(express.bodyParser()); // 应该去掉

// app.js中的 
var MongoStore = require('connect-mongo'); 
// 应改成 
var MongoStore = require('connect-mongo')(express);
```

- 问题六：
```
// 问题：出现 has no method 'router' 问题，解决办法如下：
// 1. 保留原来的 app.use(app.router); 
// 2. 不要按作者的说法改成 app.use(express.router(routes));
// 3. 并且在 app.js 最末尾加上 routes(app);
// 4. 而且还要删除掉 app.js 中的
app.get('/', routes.index);
app.get('/u/:user', routes.user);
app.post('/post', routes.post);
app.get('/reg', routes.reg);
app.post('/reg', routes.doReg);
app.get('login', routes.login);
app.post('login', routes.doLogin);
app.get('/logout', routes.logout);
```

- 问题七：
```
// 问题：req.flash 方法不能用，解决办法如下：

// 1. 安装connect-flash组件
$ npm install connect-flash
 
// 2. 并在app.js中加入：
var flash = require('connect-flash');
app.use(flash());
```

- 问题八：
```
Error: Cannot use a writeConcern without a provided callback 
    at Db.ensureIndex (D:\Work\code\nodejs\microblog\node_modules\mongodb\lib\mongodb\db.js:1395:11)

// \models\user.js 中的 
collection.ensureIndex('name', {unique: true}); 
// 应改成
collection.ensureIndex('name', {unique: true}, function(err, user){});

// \models\post.js 中的 
collection.ensureIndex('user'); 
// 应改成
collection.ensureIndex('user' ,function(err, post){});
```

[MicroBlog 项目完整代码 百度网盘 下载地址](http://pan.baidu.com/s/1pJssOJ5)

在解决问题的过程中，参考了不少其他朋友的帖子，非常感谢大家：
1.[跟着《Node.js开发指南》写MicroBlog实例总结](http://blog.sina.com.cn/s/blog_6591eb240101cmwm.html)
2.[使用Express3.0实现《Node.js开发指南》中的微博系统](http://www.cnblogs.com/meteoric_cry/archive/2012/07/23/2604890.html) 
3.[观后感《Node.js开发指南》](https://cnodejs.org/topic/50367e6ff767cc9a51d2e021)      
4.[express：node throwing error on mongodb](http://www.cnblogs.com/meteoric_cry/archive/2012/07/21/2602384.html) 

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/22818121)
</div>