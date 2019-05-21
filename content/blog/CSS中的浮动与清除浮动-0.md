---
title: CSS 中的浮动与清除浮动
date: 2015-10-16 20:35:02
tags: CSS
description: [frontend]
---

浮动元素会脱离文档流并向左/向右浮动，直到碰到父元素或者另一个浮动元素。

###  两种情况
清除浮动包括清除子元素的浮动和清除上级元素的浮动，其中清除上级元素的浮动，只需设置clear为both就可以了，而清除子元素的浮动则可以用空标签法、clearfix方法或overflow方法。
因清除上级元素的浮动比较简单，而空标签法清除子元素浮动会增加额外标签，所以在这里主要说clearfix方法、overflow方法及偶然发现的inline-block方法。

<!-- more -->

### 为什么要清除浮动
一个块级元素的高度如果没有设置height，那么其高度就是由里面的子元素来撑开的，如果子元素使用浮动，脱离了标准的文档流，那么父元素的高度会将其忽略，使用firebug查看下如果不清除浮动，父元素会出现高度不够，那样如果设置border或者background都得不到正确的解析。

浮动的三个特点很重要。
>* 脱离文档流。
>* 向左/向右浮动直到遇到父元素或者别的浮动元素。
>* 浮动会导致父元素高度坍塌

```html
<ul id="demo1" class="nostyle demo clearfix">
   <li><img alt="img1" src="http://placehold.it/150/ffffff/00c5e3&amp;text=demo"></li>
   <li><img alt="img2" src="http://placehold.it/150/ffffff/00c5e3&amp;text=demo"></li>
   <li><img alt="img3" src="http://placehold.it/150/ffffff/00c5e3&amp;text=demo"></li>
</ul>
```

------
###  清除子元素浮动clearfix方法

```css
/*简洁版*/
.clearfix:before, .clearfix:after {
	content:"";
	display:table;
}
.clearfix:after{
	clear:both;
	overflow:hidden;
}
.clearfix{
    zoom:1;
}
/* 经典版 */
.clearfix:after {
    visibility: hidden;
    display: block;
    font-size: 0;
    content: " ";
    clear: both;
    height: 0;
}
* html .clearfix             { zoom: 1; } /* IE6 */
*:first-child+html .clearfix { zoom: 1; } /* IE7 */
```

---

###  清除子元素浮动overflow方法

```css
/* overflow:auto */
#demo2{
	overflow:auto;*zoom:1;
}
/*或 overflow:hidden */
#demo2{
	overflow:hidden;*zoom:1;
}
```
---
注：这种方法主要是对父元素设置css，所以不需要加个class，下面的inline-block方法相同，只需设置父元素的css即可


### 闭合浮动的原理：

以上方法，分为两类：

- 在浮动元素末尾添加一个空元素
- 设置父元素overflow 或者 display:table 来闭合浮动

为什么设置父元素overflow 或者 display:table 可以闭合浮动?
其原理为**Block formatting contexts** （块级格式化上下文），以下简称 **BFC**。
CSS3里面对这个规范做了改动，称之为： flow root ，并且对触发条件进行了进一步说明。

###  如何触发BFC?

- float 除了none以外的值
- overflow 除了visible 以外的值（hidden，auto，scroll ）
- display (table-cell，table-caption，inline-block)
- position（absolute，fixed）
- fieldset元素

### BFC的特性：

- 块级格式化上下文会阻止外边距叠加。当两个相邻的块框在同一个块级格式化上下文中时，它们之间垂直方向的外边距会发生叠加。换句话说，如果这两个相邻的块框不属于同一个块级格式化上下文，那么它们的外边距就不会叠加。
- 块级格式化上下文不会重叠浮动元素。根据规定，一个块级格式化上下文的边框不能和它里面的元素的外边距重叠。这就意味着浏览器将会给块级格式化上下文创建隐式的外边距来阻止它和浮动元素的外边距叠加。由于这个原因，当给一个挨着浮动的块级格式化上下文添加负的外边距时将会不起作用
- 块级格式化上下文通常可以包含浮动

参考文献：
> * [那些年我们一起清除过的浮动][1]
> * [【CSS】浮动和它的工作原理？清除浮动的技巧？][2]


[1]: http://www.iyunlu.com/view/css-xhtml/55.html
[2]: http://snailsky.me/2014/08/20/%E6%B5%AE%E5%8A%A8%E5%92%8C%E5%AE%83%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%EF%BC%9F%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%8A%80%E5%B7%A7%EF%BC%9F/
