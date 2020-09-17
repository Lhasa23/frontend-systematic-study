# jQuery

> 参考阮一峰：[jQuery使用指南](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)

## 介绍

jQuery曾是前端开发使用最广泛的库，优点在于jQuery提供了相当便捷且直观的DOM操作。但是由于封装的功能过于庞大，导致想要精通更好的使用jQuery也同样具有一定难度。

下面来介绍一下jQuery的简单使用方法。

## 获取元素

获取元素是DOM操作中最重要的初始行为，使用jQuery进行最简单的元素选择是使用CSS的选择器：

```text
$(document) //选择整个文档对象

$('#id') // 选择ID为id的元素

$('.class') // 选择class为class的div元素

$('input[name=first]') // 选择name属性等于first的input元素
```

还有一些jQuery特有的选择方式

```text
$('a:first') //选择网页中第一个a元素

$('tr:odd') //选择表格的奇数行

$('#myForm :input') // 选择表单中的input元素

$('div:visible') //选择可见的div元素

$('div:gt(2)') // 选择所有的div元素，除了前三个

$('div:animated') // 选择当前处于动画状态的div元素
```

通过jQuery自己封装的丰富的api，还能实现更为精确地元素获取

```text
$('div').has('p'); // 选择包含p元素的div元素

$('div').not('.myClass'); //选择class不等于myClass的div元素

$('div').filter('.myClass'); //选择class等于myClass的div元素

$('div').first(); //选择第1个div元素

$('div').eq(5); //选择第6个div元素

$('div').next('p'); //选择div元素后面的第一个p元素

$('div').parent(); //选择div元素的父元素

$('div').closest('form'); //选择离div最近的那个form父元素

$('div').children(); //选择div的所有子元素

$('div').siblings(); //选择div的同级元素
```

## 修改元素及其属性

jQuery对元素或元素的属性的值的操作封装成getter/setter模式，通过对同一个函数进行重载，来实现读取或是修改的功能

```text
以下函数含有参数时是修改元素值，不含参数时是获取元素值

.html() 取出或设置html内容

.text() 取出或设置text内容

.attr() 取出或设置某个属性的值

.width() 取出或设置某个元素的宽度

.height() 取出或设置某个元素的高度

.val() 取出某个表单元素的值
```

## 创建、删除、复制元素

创建元素只需要简单地把html代码传入jQuery的构造函数即可：`$('<p>Hello</p>');`

复制和删除元素：

```text
删除
.remove() 不保留被删除元素
.detach() 保留被删除元素
.empty() 清空选中元素

复制
.clone()
```

## 移动元素

jQuery提供成对的四对方法，来实现元素移动。每对方法中的其一是直接移动目标元素，另一是移动其他元素，最终结果都是使目标元素到达想要的位置

```text
以下这一对方法实现的最终效果都是div元素插入p元素的后面

$('div').insertAfter($('p')); // 该方法返回div元素
$('p').after($('div')); // 该方法返回p元素
```

下面列举出所有的四对方法

```text
.insertAfter()和.after()：在现存元素的外部，从后面插入元素

.insertBefore()和.before()：在现存元素的外部，从前面插入元素

.appendTo()和.append()：在现存元素的内部，从后面插入元素

.prependTo()和.prepend()：在现存元素的内部，从前面插入元素
```

## 链式操作

链式操作是jQuery最为精髓的设计，以上展示的所有函数均可以以链式操作的形式完成复杂的多步操作

```text
$('div').remove();
$('div').find('h3').eq(2).html('Hello');
```

jQuery还提供了`.end()`函数，用来使目前操作的DOM元素返回到上一次获取到的DOM元素

```text
$('div')

　.find('h3')

　.eq(2)

　.html('Hello')

　.end() //退回到选中所有的h3元素的那一步

　.eq(0) //选中第一个h3元素

　.html('World'); //将它的内容改为World
```
