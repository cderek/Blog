---
title: Javascript事件流：捕获、目标、冒泡
date: 2015-10-19 19:52:18
tags: [JavaScript事件]
description: [Javascript事件流]
---

**捕获事件流**：Netscape提出的事件流，即事件由页面元素接收，逐级向下，传播到最具体的元素。

**冒泡事件流**：IE提出的事件流，即事件由最具体的元素接收，逐级向上，传播到页面。

两种事件流在DOM2级事件模型中得到了统一，相较于DOM0级事件模型，DOM2级有着如下特点：
> * 允许在某个元素上绑定多个同类型的事件
> * 规定了事件流的三个阶段：捕获阶段、目标阶段、冒泡阶段　　


---
　　　
###事件的触发有三个阶段
- document 往事件触发地点，捕获前进，遇到相同注册事件立即触发执行
- 到达事件位置，触发事件（如果该处既注册了冒泡事件，也注册了捕获事件，按照注册顺序执行）
- 事件触发地点往 document 方向，冒泡前进，遇到相同注册事件立即触发

------
###捕获阶段（Capture Phase）

当我们在 DOM 树的某个节点发生了一些操作（例如单击、鼠标移动上去），就会有一个事件发射过去。这个事件从 Window 发出，不断经过下级节点直到目标节点。在到达目标节点之前的过程，就是捕获阶段（Capture Phase）。

所有经过的节点，都会触发这个事件。捕获阶段的任务就是建立这个事件传递路线，以便后面冒泡阶段顺着这条路线返回 Window。

监听某个在捕获阶段触发的事件，需要在事件监听函数传递第三个参数 true。

element.addEventListener(<event-name>, <callback>, true);
在使用 addEventListener 函数来监听事件时，第三个参数设置为 false，这样监听事件时只会监听冒泡阶段发生的事件。

### 目标阶段（Target Phase）

当事件跑啊跑，跑到了事件触发目标节点那里，最终在目标节点上触发这个事件，就是目标阶段。
需要注意的时，事件触发的目标总是最底层的节点。比如你点击一段文字，你以为你的事件目标节点在 div 上，但实际上触发在 <p>、<span> 等子节点上。例如：

### 冒泡阶段（Bubbling Phase）
当事件达到目标节点之后，就会沿着原路返回，由于这个过程类似水泡从底部浮到顶部，所以称作冒泡阶段。
在实际使用中，并不需要把事件监听函数准确绑定到最底层的节点也可以正常工作。


### 标准的事件监听函数

```JavaScript
element.addEventListener(<event-name>, <callback>, <use-capture>);
obj.addEventListener("click", func, true); // 捕获方式
obj.addEventListener("click", func, false); // 冒泡方式
```
---
表示在 element 这个对象上面添加一个事件监听器，当监听到有 <event-name> 事件发生的时候，调用 <callback> 这个回调函数。至于 <use-capture> 这个参数，表示该事件监听是在“捕获”阶段中监听（设置为 true）还是在“冒泡”阶段中监听（设置为 false）。
　
## 阻止事件传递（stopPropagation）停止事件冒泡　

所有的事情都会有对立面，事件的冒泡阶段虽然看起来很好，也会有不适合的场所。比较复杂的应用，由于事件监听比较复杂，可能会希望只监听发生在具体节点的事件。这个时候就需要停止事件冒泡。

停止事件冒泡需要使用事件对象的 stopPropagation 方法，具体代码如下：

```JavaScript
element.addEventListener('click', function(event) {
    event.stopPropagation();
}, false);
```

在事件监听的回调函数里，会传递一个参数，这就是 Event 对象，在这个对象上调用 stopPropagation 方法即可停止事件冒泡　　

### 事件的委托（代理 Delegated Events）的原理以及优缺点
因为事件有冒泡机制，所有子节点的事件都会顺着父级节点跑回去，所以我们可以通过监听父级节点来实现监听子节点的功能，这就是事件代理。

###优点：
> * 减少事件绑定，提升性能。之前你需要绑定一堆子节点，而现在你只需要绑定一个父节点即可。减少了绑定事件监听函数的数量。
> * 动态变化的 DOM 结构，仍然可以监听。当一个 DOM 动态创建之后，不会带有任何事件监听，除非你重新执行事件监听函数，而使用事件监听无须担忧这个问题。

###缺点：
事件代理的应用常用应该仅限于上述需求下，如果把所有事件都用代理就可能会出现事件误判，即本不应用触发事件的被绑上了事件。

```JavaScript
var toolbar = document.querySelector(".toolbar");
toolbar.addEventListener("click", function(e) {
  var button = e.target;
  if(!button.classList.contains("active"))
    button.classList.add("active");
  else
    button.classList.remove("active");
});
```

### 通过事件委托方式

```JavaScript
$('ul').on('click', 'li', function(e) {
    if(e.target.tagName === 'LI') {
        // event handler
    }
});
```


![cmd-markdown-logo](http://jiangshui.b0.upaiyun.com/blog/2014/12/event0.svg)

------

参考文献：
> * [JavaScript 和事件][1]

[1]: http://yujiangshui.com/javascript-event/
