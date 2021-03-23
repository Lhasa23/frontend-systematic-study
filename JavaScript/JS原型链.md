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

思索了很久不知道如何用语言描述原型链，我们用一张图来看看。

![原型链](./prototype.png)

图中用虚线箭头连起来的地方，都是地址相同（指向同一片内存），箭头两端使用`===`进行判定的话，输出结果为`true`。

使用变量声明创建数组或对象时，都是隐式调用了`new Array()`或`new Object()`，也就是使用了Array或Object的构造函数生成数组或对象。

>以下都使用数组举例

我们都知道声明的数组变量自带`push()`等api，而我们声明新数组的时候并没有给它定义这些方法，那么这些共有方法就是从原型上继承来的。

使用`new XXX()`创建任何对象时，都会把`XXX()`自身的`prototype`的地址赋值给新生成的对象的`__proto__`属性，这样一来，原型链就连接起来了，换一种说法，子类继承了父类的共有方法。

我们在使用新数组的`push()`方法时，引擎发现新数组并没有定义这一方法，就沿着`__proto__`向上寻找，结果只找一次找到了`Array`头上，就发现`Array`的原型`prototype`上存在`push()`方法，于是这一次方法的调用就这么愉快的结束了。

如果是使用构造函数创建的多次继承子对象，可能要沿着多级父对象的`__proto__`找到带头大哥`Object`头上，这样一次方法的调用才算结束。

## 继承的写法

1. 使用构造函数

```js
function Animal(type, name) {
    this.type = type
    this.name = name
}
Animal.prototype.bark = () => {
    if (this.type === 'dog') {
        return '汪汪汪'
    } else {
        return '喵喵喵'
    }
}

let kittey = new Animal('cat', 'kittey')
let wangcai = new Animal('dog', 'wangcai')
kittey.bark() // '喵喵喵'
wangcai.bark() // '汪汪汪'
```

2. 使用class关键字

```js
class Animal {
  constructor(type, name){
      this.type = type
      this.name = name
  }
  bark() {
      if (this.type === 'dog') {
          return '汪汪汪'
      } else {
          return '喵喵喵'
      }
  }
}
let kittey = new Animal('cat', 'kittey')
let wangcai = new Animal('dog', 'wangcai')
kittey.bark() // '喵喵喵'
wangcai.bark() // '汪汪汪'
class Human extends Animal {
  constructor(type, name, age) {
      super(type, name); // 记得用super调用父类的构造方法!
      this.age = age;
  }

  happyBirthday() {
      return 'my name is ' + this.name + ', i am ' + this.age + ' years old.'
  }
}
let jhon = new Human('human', 'jhon', 18)
jhon.bark() // 哈哈哈约翰居然喵喵叫，就是因为继承了哺乳动物的bark()方法
jhon.happyBirthday() // my name is jhon, i am 18 years old.
```

> 以上就是本篇回忆的全部内容啦~
