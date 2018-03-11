title: JavaScript的__proto__属性介绍
date: 2015-04-12 11:29:00
comments: true
categories: [技术,JavaScript]
tags: [JavaScript,原型链,prototype]
---

在 JavaScript 中我们会约定俗成，如果一个方法是被 new 出来使用的，那么该方法名首字母通常会大写，例如下面代码块中的 Person。（我们也可以把 Person 看成 Java 或 C# 中的类）

``` javascript
var Person = function(name) {
  this.name = name;
}
```

在 JavaScript 中一个类被 new 出来的具体过程如下：
``` javascript
// 初始化一个对象 p
var p = new Person();
```

``` javascript
// 初始化一个对象 p
var p = {};
p.__proto__ =  Person.prototype;
Person.call(p);
```

以上两段代码的作用完全一样，关键在第2段代码的第2行，我们来证明一下：

<!-- more --> 

``` javascript
var p = new Person();
console.log(p.__proto__ ===  Person.prototype); // true
```

上面这段代码返回的true，说明我们第2段代码是正确的。

#### 那么 \_\_proto\_\_ 是什么？

- 简单来说，在 JavaScript 中每个对象都会有一个 \_\_proto\_\_ 属性，当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么他就会去 \_\_proto\_\_ 里找这个属性，这个 \_\_proto\_\_ 又会有自己的 \_\_proto\_\_，于是就这样一直找下去，也就是我们平时所说的原型链的概念。

- 按照标准，\_\_proto\_\_ 是不对外公开的，但是 chrome 的引擎却将他暴露了出来成为了一个公有属性，我们可以对其进行访问和赋值。但 ie 浏览器是不能访问这个属性的，所以不推荐大家直接操作这个属性，以免造成浏览器兼容问题。

![](http://qn.shisb.com/blog/javascript-introduction-proto/1.jpg) 

概念说清楚了，让我们看段代码：

``` javascript
var Person = function() {};
Person.prototype.say = function() {
  alert("Person say");
}
var p = new Person();
p.say(); // Person say
```

这段代码很简单，相信每个人都这样写过，那就让我们看下为什么 p 可以访问 Person 的 say。它等同于下面这段代码：

``` javascript
var Person = function() {};
Person.prototype.say = function() {
  alert("Person say");
}
var p = {};
p.__proto__ =  Person.prototype;
Person.call(p);
p.say(); // Person say
```

当我们调用 p.say() 时，p 中是没有 say 属性，于是到 p 的 \_\_proto\_\_ 属性中去找，也就是 Person.prototype，而我们定义了 Person.prototype.say=function(){}; 于是，p 在 Person.prototype 中就找到了这个方法。
过程：p --> p.\_\_proto\_\_ === Person.prototype

接下来，让我们看个更复杂的代码。

``` javascript
var Person = function() {};
Person.prototype.say = function() {
  console.log("Person say");
}
Person.prototype.salary = 50000;

var Programmer = function() {};
Programmer.prototype = new Person();
Programmer.prototype.writeCode = function() {
  console.log("Programmer writes code");
};
Programmer.prototype.salary = 500;

var p = new Programmer();
p.say(); // Person say
p.writeCode(); // Programmer writes code
console.log(p.salary); // 500
```

大家先不要看下面的推导过程，自己试着推导一下。

``` javascript
var Person = function() {};
Person.prototype.say = function() {
  console.log("Person say");
}
Person.prototype.salary = 50000;

var Programmer = function() {};
Programmer.prototype = new Person();
// 推导过程 --> 
// Programmer.prototype = {};
// Programmer.prototype.__proto__ = Person.prototype;
// Person.call(Programmer.prototype);

Programmer.prototype.writeCode = function() {
  console.log("Programmer writes code");
};
Programmer.prototype.salary = 500;

var p = new Programmer();
// 推导过程 --> 
// var p = {};
// p.__proto__ = Programmer.prototype;
// p.__proto__ = new Person();
// p.__proto__.__proto__ = Pserson.prototype;
// Person.call(p.__proto__);
// Programmer.call(p);

p.say(); // Person say
p.writeCode(); // Programmer writes code
console.log(p.salary); // 500

```

当我们调用 p.say() 时，p 中是没有 say 属性，于是到 p 的 \_\_proto\_\_ 属性中去找，也就是 Programmer.prototype，此时 Programmer.prototype 是等于 new Person()，但 new Person() 中也没有 say 属性，于是又到 new Person().\_\_proto\_\_ 中找，此时  new Person().\_\_proto\_\_ 等于 Pserson.prototype 的，我们定义了 Person.prototype.say=function(){}; 所以，p 在 Person.prototype 中就找到了这个方法。
过程：
p --> 
p.\_\_proto\_\_ === Programmer.prototype === new Person() --> 
p.\_\_proto\_\_.\_\_proto\_\_ === Programmer.prototype.\_\_proto\_\_ === new Person().\_\_proto\_\_ === Pserson.prototype

调用 p.writeCode() 和 p.salary 也都是同样的原理，大家自己推导。

>重要的事情说三遍！！！
>只要大家理解了下面段代码，并且记住了，就能轻而易举的掌握 JavaScript 的原型链知识。
>只要大家理解了下面段代码，并且记住了，就能轻而易举的掌握 JavaScript 的原型链知识。
>只要大家理解了下面段代码，并且记住了，就能轻而易举的掌握 JavaScript 的原型链知识。

``` javascript
var Person = function(name) {
  this.name = name;
}

// 初始化一个对象 p
var p = new Person();

// 推导过程 --> 
// var p = {};
// p.__proto__ =  Person.prototype;
// Person.call(p);
```

参考资料：[关于\_\_proto\_\_和prototype的一些理解](http://www.cnblogs.com/zzcflying/archive/2012/07/20/2601112.html)

<div class="article-statement">
本文最初发表于CSDN博客，现已迁到 [石佳劼的博客](/)，感谢CSDN。[原文链接](http://blog.csdn.net/shi0090/article/details/45008595)
</div>