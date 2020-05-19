# 朝花夕拾-CSS常用技巧总结

## 文档流

文档流是指html元素按从上到下，从左到右的默认顺序在网页展开。

CSS的一些属性会导致文档脱离文档流：
    1. `float`
    2. `position: absolute/fixed;`

## 盒模型

css盒模型包含两种，`content-box`和`border-box`。

两种盒模型在不加宽度时在表现上完全相同，加了宽度之后将会展现出不同之处。

在设置宽度时的差异：(用户设置的width用大写形式WIDTH来区分)

  1. content-box：设置的仅是content的宽度，当整个盒模型计算宽度时，计算规则为：`border width * 2 + padding width * 2 + WIDTH(content width)`

  2. border-box：设置的是盒模型border以内所有内容的宽度和（border，padding，content），计算规则为，`WIDTH = border width * 2 + padding width * 2 + content width`

一般开发中更常用`border-box`，使用`box-sizing: border-box;`来指定。

## css布局

### float

float布局是IE时代最常用的布局方式，最初目的用于使文字浮动于图片周围。后来被拿来做各种页面格式的灵活布局。

使用float布局的话一定要谨防父元素高度塌陷问题，只要使用浮动，就必须在父元素加三句话来清除浮动（一般使用单独的类来封装）

```css
.clearfix::after {
    content: '';
    display: block;
    clear: both;
}
```

### flex布局

flex最粗浅的应用，实现水平垂直居中布局

在曾经的css中实现水平垂直居中布局还需要指定position，top，left，translate；使用flex布局只使用两句话`justify-content:centent;align-items:center;`即可实现。

[flex MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex)

### grid布局

grid网格布局可以简单的看做最新的表格布局，但功能与便利性远超已经被淘汰的表格布局，但浏览器支持度不一，可以说是面向未来的布局方式。

[grid MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/grid)

## 定位

CSS使用position进行定位，position的值有`static | relative | absolute | sticky | fixed`

[CSS position定位 MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)

### CSS的样式的主要学习途径就是不停的用，观察展示效果，不停的查MDN。

## 浏览器渲染CSS原理

1. 浏览器在收到资源时会依次解析：  
    1. HTML
    2. CSS
    3. JS
2. 解析完成后，浏览器引擎会通过DOM Tree 和 CSS Rule Tree 来构造 Rendering Tree。
3. 浏览器引擎解析CSS样式表，构建CSSOM来渲染样式：
    1. 首先要做的是识别出Token，然后构建节点并生成CSSOM。
      ![从css构建CSSOM](https://github.com/Lhasa23/my-image-repo/blob/master/CSSOM.jpg)
    2. CSS匹配HTML元素是一个相当复杂和有性能问题的事情。所以，CSSOM树要小，CSS尽量用id和class，千万不要过渡层叠下去。
4. 构建渲染树：  
    当我们生成 DOM 树和 CSSOM 树以后，就需要将这两棵树组合为渲染树。  
    在这一过程中，渲染树只会包括需要显示的节点和这些节点的样式信息，如果某个节点是 display: none 的，那么就不会在渲染树中显示。
5. 布局与绘制：  
    当浏览器生成渲染树以后，就会根据渲染树来进行布局（也可以叫做回流）。这一阶段浏览器要做的事情是要弄清楚各个节点在页面中的确切位置和大小。通常这一行为也被称为“自动重排”。  
    布局流程的输出是一个“盒模型”，它会精确地捕获每个元素在视口内的确切位置和尺寸，所有相对测量值都将转换为屏幕上的绝对像素。  
    布局完成后，浏览器会立即发出“Paint Setup”和“Paint”事件，将渲染树转换成屏幕上的像素。

## CSS动画

### transition

`transition`可以为一个元素在不同状态之间切换的时候定义不同的过渡效果。可以在元素增加`:hover`和`:active`伪类来实现动画效果。

`transition: property duration timing-function delay;`

不同组之间的过渡可以使用逗号进行分隔，例如：  
`transition: margin-right 4s, color 1s;`

### animation

`animation`是css动画效果，属性有`duration | timing-function | delay | iteration-count | direction | fill-mode | play-state | name`；其中`name`属性的值为使用`@keyframes`定义的transform效果，在整个周期的关键帧点进行规定，其余帧由浏览器自动填补。

```css
@keyframes sliding {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
```
