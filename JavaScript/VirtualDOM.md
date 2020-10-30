# Virtual DOM

## 虚拟DOM是什么？

直白的说，一个虚拟DOM就是一个JS对象，编译阶段通过固定的逻辑（不同框架对虚拟DOM的解析方法不同），将这个对象解析，并生成真实的DOM节点到页面上。

虚拟DOM最显而易见的优势是，使用DOM diff算法将js操作大量DOM节点的时间损耗优化，一次修改所有产生变化的节点。另一方面，虚拟DOM对跨平台开发成为可能，虚拟DOM的本质就是一个js对象，使用不同的解析方式甚至可以把虚拟DOM解析为安卓和IOS的原生组件。

下面举个例子：

```html
<div>
    <p class="text">这里是一段话</p>
</div>
```

转化为虚拟DOM可能是下面这样：

```js
{
    tag: 'div',
    children: [
        {
            tag: 'p',
            props: {
                class: 'text'
            },
            children: [
                {text: '这里是一段话'}
            ]
        }
    ]
}
```

## 虚拟DOM的优缺点

### 虚拟DOM的优点在于DOM diff(减少DOM操作)和跨平台

* 减少DOM操作

    例如：

    ```js
    // 以下伪代码
    // 用DOM API生成1000个div
    for (let i = 1; i <= 1000; i++) {
        const div = document.createElement('div')
        div.innerText = i
        document.querySelector('#app').append(div)
    }
    // 以上方法是一个一个生成div元素，又一个一个把div append到页面里

    // 使用虚拟DOM的话，会一次性生成1000个虚拟DOM，然后把这1000个节点一次性生成为1000个div节点，并一次性加载到页面中
    // Vue
    // <div v-for="item in 1000">{{item}}</div>
    // React
    // return (
    //     {[1,...,1000].map(item => <div>{item}</div>)}
    // )
    ```

* 跨平台

    跨平台的优点就如上述所说，虚拟DOM的本质就是一个js对象，使用不同的解析方式甚至可以把虚拟DOM解析为安卓和IOS的原生组件。

### 虚拟DOM的缺点

虚拟DOM的缺点限于需要特定的解析函数，例如Vue的`h()`和React的`createElement()`。如果难点算一个缺点，就是新手使用时因为不够了解DOM diff的特点，所导致的删除时出错。

## DOM diff

DOM是树形结构，又因为虚拟DOM是js对象的本质，虚拟DOM同样可以转化为树结构。

而DOM diff，顾名思义，就是对虚拟DOM进行对比的算法（一般来说是一个patch函数）。通过对比新旧虚拟DOM树的每一个对象节点，每次找出变化的节点，把该节点的变化记录下来，直到遍历完整棵树，再仅把记录下的产生变化的节点重新渲染到页面，节省大量资源。

### diff的过程及过程中的bug(key的重要性)

如下代码：

```html
<div>
    <span v-for="item in ['1','2','3']">
        {{item}}
        <button>删除</button><!-- 删除当前span -->
    </span>
</div>
```

生成的虚拟DOM树为：

![initial.png](https://github.com/Lhasa23/my-image-repo/blob/master/initial.png)

这时删掉第二个span，我们获得的虚拟DOM树为：

![expect.jpg](https://github.com/Lhasa23/my-image-repo/blob/master/expect.jpg)

但diff算法实际上进行的操作为，删掉第三个span，将第二个span的值从2改为3：

![infact.jpg](https://github.com/Lhasa23/my-image-repo/blob/master/infact.jpg)

有没有发现bug？

这时我们需要借助key，来标识每一个独特的虚拟DOM节点：

![initialKey.jpg](https://github.com/Lhasa23/my-image-repo/blob/master/initialKey.jpg)

这时我们删除第二个span，diff算法发现删掉的确确实实是key=2的span节点：

![infactKey.jpg](https://github.com/Lhasa23/my-image-repo/blob/master/infactKey.jpg)

如果需要key来标识不可复用节点，那么key同样不要使用index作为标识，一定要使用类似于id这样不会改变局部唯一的变量。