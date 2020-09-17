# MVC入门

## 什么是MVC

M：Model（数据模型）  
V：View（视图）  
C：Controller（控制器）

M是数据终端，用于保管所有的数据；V算是视图终端，从M那里获取数据控制页面所有的视图；C是控制终端，控制页面的全部行为，包括数据的行为，页面的逻辑等。

```js
// M
const m = {
    data() {}, // 全部数据
    add() {}, // 增删改查
    edit() {},
    delete() {}
}
// V
const v = {
    el: '', // 挂载目标
    template: '', // 填充元素
    render() {} // 元素渲染
}
// C
const c = {
    onChange() {}, // 绑定页面事件等等
    ...others
}
```

通过MVC设计模式解耦传统网页，使前端代码更易维护，结构清晰，耦合度低，稳定性更高。

## EventBus

eventBus是一种事件总线，事件产生时（`eventBus.$emit`）通过总线传递相关信息，其他方法中使用总线的API来获取目标事件的信息（`eventBus.$on`）。因为所有事件的产生和管理都在这一根线上，类似于电路系统，事件产生类似于触发了电位变化，外接的线路就可以获取到电位的变化，从而进行相关操作。

```js
// 绑定事件
eventBus.$on('eventName', cb)
// 触发事件
eventBus.$emit('eventName', arguments)
```

## 表驱动编程

表驱动编程应用了hashMap的思维模式，使用相应的key值存储相应的数据（变量，函数，对象等等），将定义与使用解耦，通过共有的hashMap，使用特定的key就能获取想要的数据。对于数据统一化管理提供了质变的好处。

```js
const nameScore = {
    'xiaoming': 90,
    'xiaohong': 60
}
for (let name in nameScore) {
    console.log(nameScore[name])
}
```

## 模块化

模块化是：  
* 将一个复杂的程序依据一定的规则(规范)封装成几个块(文件), 并进行组合在一起
* 块的内部数据与实现是私有的, 只是向外部暴露一些接口(方法)与外部其它模块通信

模块化具有：
* 避免命名冲突(减少命名空间污染)
* 更好的分离, 按需加载
* 更高复用性
* 高可维护性

的优点