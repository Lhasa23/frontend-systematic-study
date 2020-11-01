## DOm事件模型

先来看一段代码：

```html
<div class="grandfather">
    <div class="father">
        <button class="son"></button>
    </div>
</div>
<script>
    document.querySelector('.son').addEventListener('click', fn, [third parameter]) // 先搁置第三个参数
</script>
```

当我们点击button时，点击事件会被触发，这时有两个选择：
   1. 最开始`.grandfather`监听到点击事件，然后是`.father`，最后是`.son`监听到事件，然后执行了函数fn
   2. 最开始是`.son`监听到事件，然后执行了函数fn，接下来`.father`监听到了点击事件，最后是`.grandfather`监听到点击事件

上述过程1：从外向内依次获取到点击事件触发的信息，这个过程叫做——事件捕获

上述过程2：从内向外一次获取到点击事件触发的信息，这个过程叫做——事件冒泡

![捕获与委托](https://github.com/Lhasa23/my-image-repo/blob/master/%E6%8D%95%E8%8E%B7%E4%B8%8E%E5%A7%94%E6%89%98.jpg)

但是这样的话，函数fn到底是在过程1中执行呢？还是在过程2中执行？

这时就跟我们在上面搁置的第三个参数有关了，第三个参数名为`useCapture`(最常用的，第三个参数实际上还能传一个配置对象`options`，详细可以在MDN了解)，接受一个boolean值，因此顾名思义：如果传递的值为`true`，fn函数就会在捕获阶段执行；不传或传值为`false`，fn则会在冒泡阶段执行。

![事件函数](https://github.com/Lhasa23/my-image-repo/blob/master/%E4%BA%8B%E4%BB%B6%E5%87%BD%E6%95%B0.jpg)

### 事件机制

这时候又涉及到**事件机制**的问题，捕获和冒泡是按什么顺序执行呢？

W3C的规范里，事件发生要先从外到内依次捕获，接下来从内向外依次冒泡；而`addEventListener`只能在某一个阶段监听，不能又在捕获阶段监听并执行，又想在冒泡阶段监听并执行。

### event

`addEventListener`触发的事件event，会被做为回调函数fn的第一个参数传给回调函数fn。假设我们把事件监听注册在`.grandfather`上，这时点击`.son`，`event.target`就是`.son`的元素，`event.currentTarget`是监听事件注册的地方——即`.grandfather`

### e.stopPropagation()

事件捕获无法取消，但我们可以亡羊补牢——阻止事件冒泡，在回调函数fn里使用`event.stopPropagation()`即可阻止注册监听的元素继续把事件向上冒泡。

## 事件委托

如果遇见了这样的代码：

```html
<div class="grandfather">
    <div class="father">
        <button class="son1"></button>
        <button class="son2"></button>
        <button class="son3"></button>
        <button class="son4"></button>
        <button class="son5"></button>
        <button class="son6"></button>
    </div>
</div>
```

每一个son执行类似的功能，难道我们要对每一个son都写一个事件监听`addEventListener`么？如果你的回答是：没错我就要一个一个写，那么假如有100个son，我们还能继续不怕麻烦一个一个注册事件监听么？

这时，就需要用到**事件委托**的思想：将监听事件注册到祖先元素(如`.grandfather`或`.father`)，到监听到事件触发的时候，通过`event.target`获取操作的元素及相关信息，进行区别化判断，进行区别化处理，是不是比写100，1000个`addEventListener`更容易了？

这样做还有一个好处，使用JS操作DOM在网页运行中的某一时刻生成一个button，我们是没办法在最开始就注册这个button的事件监听的。使用**事件委托**，可以在祖先借点注册监听，然后轻松监听到后来才生成的元素。
