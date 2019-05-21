---
title: JavaScript闭包与应用场景
date: 2014-12-03 12:20:37
tags: [JavaScript, 闭包]
description: [闭包及其应用场景]
---

闭包（Closure）是词法闭包（Lexical Closure）的简称。
当一个函数即便在离开了它的**词法作用域**(Lexical Scope)的情况下，仍然存储并可以存取它的**词法作用域**(Lexical Scope)，这个函数就构成了闭包。


> * 闭包是函数和引用环境组成的整体，是函数的局部变量集合，只是这些局部变量在函数返回后会继续存在
> * 闭包就是就是函数的“堆栈”在函数返回后并不释放，我们也可以理解为这些函数堆栈并不在栈上分配而是在堆上分配
> * 当在一个函数内定义另外一个函数就会产生闭包


ECMAScript闭包模型
在ECMAscript的脚本的函数运行时，每个函数关联都有一个执行上下文场景(Execution Context) ，这个执行上下文场景中包含三个部分
> * 词法环境（The LexicalEnvironment）
> * 变量环境（The VariableEnvironment）
> * this绑定
词法环境中用于解析函数执行过程使用到的变量标识符。我们可以将词法环境想象成一个对象，该对象包含了两个重要组件，环境记录(Enviroment Recode)，和外部引用(指针)。环境记录包含包含了函数内部声明的局部变量和参数变量，外部引用指向了外部函数对象的上下文执行场景。全局的上下文场景中此引用值为NULL。这样的数据结构就构成了一个单向的链表，每个引用都指向外层的上下文场景。

![cmd-markdown-logo](http://coolshell.cn//wp-content/uploads/2012/03/closure.png)

------

```JavaScript
"use strict";
var myClosure = (function outerFunction() {
  var hidden = 1;
  return {
    inc: function innerFunction() {
      return hidden++;
    }
  };
}());
myClosure.inc();  // 返回 1
myClosure.inc();  // 返回 2
myClosure.inc();  // 返回 3
```

---

## 优点

- 希望一个变量长期驻扎在内存中
- 避免全局变量的污染
- 私有成员的存在

## 缺点

- 空间浪费
- 内存泄漏
- 性能消耗

### 1. 内存空间浪费
内存浪费不仅是因为闭包使得函数中的变量都被保存在内存中，造成内存常驻，还有可能是对闭包使用不当造成无效内存产生。所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露，由于IE的js对象和DOM对象使用不同的垃圾收集方法，因此闭包在IE中会导致内存泄露问题。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

### 2. 性能消耗
闭包会在父函数外部，改变父函数内部变量的值。所以，如果把父函数当作对像（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

## 应用场景

* 封装私有变量和方法
* 保护函数内的变量安全：如迭代器、生成器
* 代码模块化，避免全局变量污染

## 常见错误

### 循环闭包
给多个元素循环绑定事件，用for循环时，不会成功，因为当第一次执行时，i就已经变成最大值，所以不能成功绑定每个元素
### 解决办法
用闭包方法，在for循环内使用立即执行函数，!function(i){/*执行函数*/}(i);。


参考文献：
> * [JavaScript 闭包详解][1] 
> * [JavaScript闭包的底层运行机制][2] 
> * [闭包的概念、形式与应用][3] 

[1]: http://codethoughts.info/javascript/2015/05/22/javascript-closure-inside-out/
[2]: http://blog.leapoahead.com/2015/09/15/js-closure/
[3]: https://www.ibm.com/developerworks/cn/linux/l-cn-closure/
