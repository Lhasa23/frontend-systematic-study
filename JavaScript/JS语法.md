# 朝花夕拾——JavaScript语法

JS基本语法与绝大部分编程语言的基本语法类似，例如表达式，语句，标识符，if else，for while等等

## 表达式和语句

表达式可以得出一个输出值，而语句仅仅是执行一个操作。

例如：  
表达式

```text
1 + 3
console.log(3)

特殊：
下面是一个对象字面量，是一个表达式，输出一个对象。
{
    foo: bar(3, 5)
}
```

语句

```text
var a = 1
或
if else语句等
```

## 标识符

标识符是代码中用来标识变量、函数、或属性的字符序列。

标识符只能包含字母或数字或下划线（“_”）或美元符号（“$”），且不能以数字开头。

例如

```text
var abc
var $abc
var a123
var _1abc
```

## 条件语句

条件语句中最常用的是`if else`语句，在大部分编程语言中都有；  
其次是`switch case`语句，在一些独特的场景需要使用，在每一个case中需要break（继承较老的编程语言的特性）；  
三元表达式`true or false ? something : another`；
短路表达式：`a && b`或`a || b`，前方的表达式若不能决定语句的真值，则继续执行后方的表达式。

## 循环语句

`while(a){b}`while语句中a表达式真值为true时，将循环执行b，一旦a真值为false，将会立刻退出循环；  
`for`语句与`while`语句类似，都是判断值为真是循环执行循环体。

`break`表示退出循环；  
`continue`表示退出当前循环；  
`break continue`可以和标记语句结合使用：

标记语句的循环非常罕见，下例中的`loop1`为label标记

```js
let str = '';

loop1:
for (let i = 0; i < 5; i++) {
  if (i === 1) {
    continue loop1;
  }
  str = str + i;
}

console.log(str);
// expected output: "0234"
```