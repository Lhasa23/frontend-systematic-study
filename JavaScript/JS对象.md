# 朝花夕拾——JavaScript对象

## 声明一个对象

1. 最常用：使用对象字面量来声明一个对象

    示例

    ```js
    let obj = {
        name: 'eleven',
        age: 24
    }
    ```

2. 常规方法

    示例

    ```js
    let obj = new Object({
        name: 'eleven',
        age: 24
    })
    ```

其实两种方法本质一样，第一种不需要显示的声明一个new Object，写法上简便了不少，所以绝大多数情况都采用第一种字面量的声明方法声明一个对象。

**注：对象的属性值都是`string`，声明中的属性名省略了引号，以指定属性名为string类型（严格的写法应为'name': 'eleven'）。**

## 删除对象的属性

`delete obj.xxx`或`delete obj['xxx']`

该语句的作用是删除对象obj中的属性xxx，该操作的所有情况的返回值都是true（**意味着如果删除一个不存在的属性也会返回true**），只有在删除的属性是一个**自身**且**不可配置**的属性时，该操作将会返回false。

因此不能用该语句看出是否删除成功，应该用`'xxx' in obj`来验证对象中是否存在目标属性，若不存在，该语句将会返回false。

## 查看对象的属性

`obj.xxx`或`obj['xxx']`

该表达式用于读对象的xxx属性，xxx是属性名，是string类型的值。

注意区别`obj[xxx]`(方括号内的xxx不带引号！！)，这种情况下，xxx可以是变量名，表达式等等。这时js会先求出xxx的值(假设xxx => 'name')，最终`obj[xxx]`就会变成`obj['name']`。

## 修改或增加对象的属性

新增或修改属性值都可以使用同一套操作来完成

```js
let obj = {
    name: 'elevel'
}
// 新增
obj.age = 18
// 修改
obj.age = 24

// 批量新增或修改
let temp = {
    a: 'a',
    b: 'b',
    c: 'c'
}
Object.assign(obj, temp)
```

## 'xxx' in obj和obj.hasOwnProperty('xxx') 的区别

`'xxx' in obj`用于验证obj对象中是否含有xxx属性，但是该操作将会按原型链向上寻找，例如：

```js
let obj = {
    name: 'eleven'
}
console.log('toString' in obj) // true
```

`obj.hasOwnProperty('xxx')`该方法只会在obj对象本身中寻找xxx属性，不会沿原型链向上寻找，例如：

```js
let obj = {
    name: 'eleven'
}
console.log(obj.hasOwnProperty('toString')) // false
console.log(obj.hasOwnProperty('name')) // true
```

## 挖坑 defineProperty()
