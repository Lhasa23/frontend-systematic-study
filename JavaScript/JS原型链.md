# 朝花夕拾——JavaScript原型链

先来看看与原型链和继承有关的定义：

每个实例对象（ object ）都有一个私有属性（称之为 `__proto__` ）指向它的构造函数的原型对象（prototype ）。该原型对象也有一个自己的原型对象( `__proto__` ) ，层层向上直到一个对象的原型对象为 null。根据定义，null 没有原型，并作为这个原型链中的最后一个环节。

几乎所有 JavaScript 中的对象都是位于原型链顶端的 Object 的实例。

有点难以理解，让我们举例来看

## 从JS中的一切对象的诞生说起 (构造函数)

> 以下代码均由Chrome控制台提供支持~

我们都知道，JS中的函数（`function`）也是一种特殊的对象，它是由函数类型（`Function`）所构造的，我们在控制台进行简单的验证

```js
function fn1() {console.log('111')}
fn1.constructor === Function // true
```

而我们可能不知道的是，函数类型（`Function`）同时也是它自身的构造者

```js
Function.constructor === Function // true
```

我们都知道，JS中的对象（`object`举例为`let o = {a:1}`）是由对象类型（`Object`）所构造的

```js
let o = {a:1}
o.constructor === Object // true
```

我们可能不知道的是，对象类型（`Object`）居然是由函数类型构造的

```js
let o = {a:1}
o.constructor.constructor === Function // true
Object.constructor === Function // true
Object.constructor === Function.constructor // true 没想到吧~但由简单推理可得该结论哦~
```

## 原型链

### 原型是什么
