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


CSS主要学习途径就是不停的用，不停的查MDN。
