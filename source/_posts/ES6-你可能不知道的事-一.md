---
title: ES6 你可能不知道的事(一)
date: 2017-11-29 16:42:50
categories:
    - programming
tags:
    - es6
---
http://taobaofed.org/blog/2016/07/22/es6-basics/
作者: 化辰  发表于: 2016-07-22

ES6 你可能不知道的事 - 基础篇

序

ES6，或许应该叫 ES2015（2015 年 6 月正式发布），对于大多数前端同学都不陌生。

首先这篇文章不是工具书，不会去过多谈概念，而是想聊聊关于每个特性 你可能不知道的事，希望能为各位同学 正确使用 ES6，提供一些指导。

对于 ES6，有些同学已经在项目中有过深入使用了，有些则刚刚开始认识他，但不论你是属于哪一类，相信这篇文章都有适合你的部分。针对文章中的问题或不同意见，欢迎随时拍砖、指正。

正文

Let + Const

这个大概是开始了解 ES6 后，我们第一个感觉自己完全明白并兴致勃勃的开始使用的特性。

以如下方式使用的同学请举下手？

// 定义常量
const REG_GET_INPUT = /^\d{1,3}$/;

// 定义配置项
let config = {
  isDev : false,
  pubDir: './admin/'
}

// 引入 gulp
let gulp    = require('gulp');

// 引入gulp相关插件
let concat  = require('gulp-concat');
let uglify  = require('gulp-uglify');
let cssnano = require('gulp-cssnano');
很多人看完概念之后，第一印象都是：“const 是表示不可变的值，而 let 则是用来替换原来的 var 的。”

所以就会出现上面代码中的样子；一段代码中出现大量的 let，只有部分常量用 const 去做定义，这样的使用方式是错误的。

你可能不知道的事

const 的定义是不可重新赋值的值，与不可变的值(immutable value)不同；const 定义的 Object，在定义之后仍可以修改其属性。

所以其实他的使用场景很广，包括常量、配置项以及引用的组件、定义的 “大部分” 中间变量等，都应该以const做定义。反之就 let 而言，他的使用场景应该是相对较少的，我们只会在 loop(for，while 循环)及少量必须重定义的变量上用到他。

猜想：就执行效率而言，const 由于不可以重新赋值的特性，所以可以做更多语法静态分析方面的优化，从而有更高的执行效率。

所以上面代码中，所有使用 let 的部分，其实都应该是用 const 的。

Template Strings（字符串模板）

字符串模板是我刚接触ES6时最喜欢的特性之一，他语法简洁，语义明确，而且很好的解决了之前字符串拼接麻烦的问题。

因为他并不是 “必须” 的，而且原有的字符串拼接思维根深蒂固，导致我们很容易忽视掉他。

使用实例

我们先来看看他的一般使用场景：

const start = 'hi all';

const getName = () => {
  return 'jelly';
};

const conf = {
  fav: 'Coding'
};

// 模板
const msg = `${start}, my name is ${getName()}, ${conf.fav} is my favourite`;
你可能不知道的事

// 1. 与引号混用
const wantToSay = `I'm a "tbfed"`;

// 2. 支持多行文本
const slogan = 
`
I have a dream today!
`;

// 比较适合写HTML
const resultTpl = 
`
  <section>
    <div>...</div>
  </section>
`;
Enhanced Object Literals（增强的对象字面量）

增强的对象字面量是 ES6 中的升华功能，他设计了很多简写，这些简写不但保留了明确的语义，还减少了我们多余的代码量。

当他的使用成为一个习惯时，我们会看到自己代码变得更为优雅。

你可能不知道的事


const _bookNum = 4;

const basicConfig = {
  level: 5
}

const config = {
  // 直接指定原型对象
  __proto__: basicConfig,
  
  // 属性简写
  _bookNum,
  
  // 方法简写
  getBookNum() {
    return this.bookNum;
  }
}
Arrows and Lexical This（箭头函数）

箭头函数是ES6中的一个新的语法特性，他的用法简单，形态优雅，备受人们青睐。

大多数同学初识这个特性时，更多的仅仅用它作为函数定义的简写，这其实就有些屈才了。

// 未使用箭头函数的写法
{
  ...
  
  addOptions: function (options) {
  
    var self = this;
  
    options.forEach(function(name, opts){
      
      self[name] = self.addChild(name, opts);
      
    });
    
  } 
}

// 使用箭头函数后的写法
{
  ...
  
  addOptions: function (options) {
  
    options.forEach((name, opts) => {
      
      this[name] = this.addChild(name, opts);
      
    });
    
  } 
}
可以注意到上下两段代码的区别。

在未使用箭头函数前，我们在过程函数中使用父级 this，需要将其显式缓存到另一个中间变量中，因为过程函数有独立的 this 变量，会覆盖父级；使用箭头函数后，不但简写了一个过程函数（ forEach 的参数），还省略掉了 this 的中间变量的定义。

原因：箭头函数没有独立执行上下文（ this ），所以其内部引用 this 对象会直接访问父级。

插播：原来我们定义这个中间变量还有一个有趣的现象，就是明明千奇百怪，例如 self, that, me, _that, _me, Self...，快站出来说说你用过哪个，还是哪几个~

当然，从这块我们也可以看出，箭头函数是无法替代全部 function 的使用场景的，例如我们需要有独立 this 的函数。

你可能不知道的事

箭头函数不但没有独立 this，他也没有独立的 arguments，所以如果需要取不定参的时候，要么使用 function，要么用 ES6 的另一个新特性 rest（具体在 rest 中会有详解）。
箭头函数语法很灵活，在只有一个参数或者只有一句表达式做方法体时，可以省略相应括号。
// 完整写法
const getOptions = (name, key) => {
  ...
}

// 省略参数括号
const getOptions = key => {
  ... 
}

// 省略参数和方法体括号
const getOptions = key => console.log(key);

// 无参数或方法体，括号不能省略
const noop = () => {};
有个简单小栗子，这一灵活的语法在写连续的Promise链式调用时，可以使代码更加优雅

gitPromise
  .then(() => git.add())
  .then(() => git.commit())
  .then(() => git.log())
  .then((msg) => {
      ...
  })
  .then(() => git.push())
  .catch((err) => {
      utils.error(err);
  });
Destructuring（解构）

解构这个特性可以简单解读为分别定义，用于一次定义多个变量，常常用于分解方法返回对象为多个变量，分别使用。
使用过ES6的同学应该或多或少接触过这个特性，但是你可能不知道它如下几个用法：

你可能不知道的事

const bookSet = ['UED', 'TB fed', 'Not find'];
const bookCollection = () => {
  return {book1: 'UED', book2: 'TB fed'};
};

// 1. 解构也可以设置默认值
const {book1, book3 = 'Not find'} = bookCollection();

// 2. 解构数组时候是可以跳过其中某几项的
const [book1,,book3] = bookSet;  // book1 = 'UED', book3 = 'Not find'

// 3. 解构可以取到指定对象的任何属性，包括它包含的方法
const {length: setLength} = bookSet;  // setLength = 3
Rest + Spread

Rest 和 Spread 主要是应用 ... 运算符，完成值的聚合和分解。

你可能不知道的事


// 1. rest 得到的是一个真正的数组而不是一个伪数组
const getOptions = function(...args){
  console.log(args.join); // function
};

// 2. rest 可以配合箭头函数使用，达到取得所有参数的目的
const getOptions = (...args) => {
  console.log(args); // array
};

// 3. spread 可以用于解构时，聚合所得的值
const [opt1, ...opts] = ['one', 'two', 'three', 'four'];

// 4. spread 可以用于数组定义
const opts = ['one', 'two', 'three', 'four'];
const config = ['other', ...opts];
Classes

ES6 中实现的一个语法糖，用于简化基于原型集成实现类定义的场景。
虽然有很多人不太喜欢这个特性，认为它作为一个简单增强扩展，并没有其他语言 class 应有的特点。
但是就我自己观点来看，还是感觉这样一种写法确实比原有的原型继承的写法语义更清晰、明确，而且语法更简单。

同样，可能有些用法是你之前容易忽略掉的，在此做个补充。

你可能不知道的事


// 1. 静态变量
// ES6 的类定义实现了静态方法的定义，但静态变量呢？
// 可以用如下方式实现: 
class TbFedMembers{
  static get HuaChen(){
    return 'jelly';
  }
}
TbFedMembers.HuaChen; // "化辰"

// 2. 私有属性（私有属性有多种实现方式，只谈及其中一种）
// 闭包
const TbFedMembers = (() => {
  const HuaChen = 'jelly';
  
  return class{
    getOneMemberName(){
      return HuaChen;
    }
  };
})();
Promises

Promise 不只是一个对象、一个语法，他更是一种异步编程方式的变化
相信使用过 ES6 的同学都已经开始尝试了 Promise，甚至在不支持ES6的时候，已经开始使用一些基于 Promise 思想的开源框架。

那么我们之前用 Promise 究竟用的对么？有什么需要注意的点呢？

你可能不知道的事


// 1. 多个异步任务同时执行用 Promise.all，顺序执行使用链式调用
// Promise.all
Promise
  .all([jsBuildPromise, cssBuildPromise])
  .then(() => {
    ...
  });

// chain
jsBuildPromise
  .then(() => cssBuildPromise)
  .then(() => {
    ...
  });


// 2. Promise 的链式调用需要每一个过程返回一个 Promise 对象才能保证顺序执行
gitPromise
  .then(() => git.add())  // 正确，箭头函数简写
  .then(() => {
    git.commit(); // 错误，函数返回 undefined，会立即执行下一过程
  })
  .then(() => {
    return git.log(); // 正确
  });


// 3. Promise 需要调用 catch 方法来捕获错误，而且过程内的错误不会阻塞后续代码执行
new Promise(() => {
  f;  // not define error !
})
.catch((err) => {
  console.log(err)  // show 'f is not define'
});
console.log('error test');  // 此行可以被正常执行
结语

基础篇主要是讲了我们最常用的一些特性，后续如果大家感兴趣，还可以再来个 “进阶篇”，最后，希望文章中的部分内容可以对大家理解和使用 ES6 有所帮助。

参考资料

https://www.stackoverflow.com
https://developer.mozilla.org/en-US/docs/Web/JavaScript
https://babeljs.io/docs/learn-es2015/
https://developer.mozilla.org/en-US/docs/Web/JavaScript
https://ponyfoo.com/articles/es6-spread-and-butter-in-depth
http://12devs.co.uk/articles/promises-an-alternative-way-to-approach-asynchronous-javascript/
http://www.2ality.com/2015/01/es6-destructuring.html
http://www.datchley.name/es6-rest-spread-defaults-and-destructuring/
分享到
