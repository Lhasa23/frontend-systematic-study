# Vue3

## Vue3新特性

---

> Vue3相对于Vue2的新特性，这些新特性在我们公司未来的前端开发上会有哪些帮助，或者新特性是为了解决哪些应用场景

Vue3与Vue2的差别其实并不大，有几点比较明显的变化在于：
1. 创建项目时不再用vue/cli命令
2. `*.vue`文件的`<template></template>`中允许有多个根节点了
3. 创建Vue实例时的方法更贴近函数式
4. 良好的支持ts语言

### Composition Functions API

Vue3新加入了一个特性，组合式函数API(composition functions api)，在`*.vue`文件的`<script></script>`标签中使用`setup(){}`

`setup(){}`与Vue2时代的结构不同，也完全支持ts（不需要额外引入修饰器），在页面功能复杂的情况下，还能针对不同的功能模块进行拆分，例如：

最简单的表格+表单页面，查询逻辑和增删改逻辑可以分别用不同的对象封装，用`...`操作符解压后return

```
import { ref } from 'vue'

setup () {
  const search = {
    let condition = ref('')
    const search = () => {
      AxiosGet(url, condition).then()
    }
  }
  const edit = {
    let editDialog = ref(false)
    const openEditDialog = () => {
      AxiosPost(url, data).then()
    }
  }
  return {
    ...search,
    ...edit
  }
}
```

### 多重v-model

在Vue2的版本中，一个组件只能绑定一个v-model，想使用更多的变量需要`v-bind:[value].sync`，在组件内部使用`this.$emit('update:[value]', value)`将改变后的值传给组件调用的地方。

Vue3中的多重v-model，实际上是省略了`.sync`操作符，之前用`v-bind`绑定的变成了名正言顺的变量。

```
// 组件调用
<survey-form v-model:name="name" v-model:age="age"></survey-form>

setup () {
    const name = ref('')
    const age = ref(18)
    return {
      name, age
    }
}

// 组件定义
<input :value="name" @input="updateName($event.target.value)" />
<input :value="age" @input="updateAge($event.target.value)" />

setup(props, { emit }) {
  const updateName = value => {
    emit('update:name', value)
  }
  const updateAge = value => {
    emit('update:age', +value)
  }
  return { updateName, updateAge }
}
```

### Suspense

`<Suspense></Suspense>`自带有两个状态`slot`，分别是`default`和`fallback`，可以异步加载组件，当加载的组件条件不满足时，会展示`fallback`状态的`slot`内容，否则是`default`状态的内容

```
<template>
  <Suspense>
    <template #default>
      <div>
        加载完成
      </div>
    </template>
    <template #fallback>
      加载中...
    </template>
  </Suspense>
</template>
```

## Vue3的使用和核心插件

---

> 工具链适配状态怎么样了，包括并不限于vue-cli, vuex, vue-router 等等

### 项目创建

Vue3不再使用vue/cli进行创建项目，而是使用vite

```
$ npm init vite-app <project-name>
$ cd <project-name>
$ npm install
$ npm run dev

||

$ yarn create vite-app <project-name>
$ cd <project-name>
$ yarn
$ yarn edv
```

package.json

```
{
  "name": "vue3-first",
  "version": "0.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build"
  },
  "dependencies": {
    "vue": "^3.0.0-rc.1"
  },
  "devDependencies": {
    "@vue/compiler-sfc": "^3.0.0-rc.1",
    "vite": "^1.0.0-rc.1"
  }
}
```

main.js

```
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'

createApp(App).mount('#app')
```

App.vue

```
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>
```

### vue-router

Vue3的vue-router需要使用4.0以上版本，一般来说执行`$ npm info vue-router versions`查看全部vue-router版本后，选择一个最新版使用安装即可。

```
// 上述命令的输出
[ ...[4.0.0之前版本], '4.0.0-alpha.0', '4.0.0-alpha.1', '4.0.0-alpha.2', '4.0.0-alpha.3', '4.0.0-alpha.4', '4.0.0-alpha.5', '4.0.0-alpha.6', '4.0.0-alpha.7', '4.0.0-alpha.8', '4.0.0-alpha.9', '4.0.0-alpha.10', '4.0.0-alpha.11', '4.0.0-alpha.12', '4.0.0-alpha.13', '4.0.0-alpha.14', '4.0.0-beta.1', '4.0.0-beta.2', '4.0.0-beta.3', '4.0.0-beta.4', '4.0.0-beta.5', '4.0.0-beta.6', '4.0.0-beta.7', '4.0.0-beta.8', '4.0.0-beta.9' ]
```

使用命令`$ npm install vue-router@4.0.0-beta.9 || $ yarn add vur-router@4.0.0-beta.9`

**如何使用vue-router**

Vue3中的vue-router初始化有点不一样

```
// 使用vue-router的新方法
import { createWebHashHistory, createRouter } from 'vue-router'

const history = createWebHashHistory()
const router = createRouter({
    history,
    routes: [{
        path: '/',
        component: () => import('./views/Home.vue')
    }, {
        path: '/doc',
        component: () => import('./views/Doc.vue')
    }]
})
export default router
```

然后将导出的`const router`加入main.js

```
createApp(App).use(router).mount('#app')
// 以下是上条语句的详细写法
// const app = createApp(App)
// app.use(router)
// app.mount('#app')
```

### vuex

Vue3也取代了Vuex，还记得前面说的Vue3的新特性 组合式函数API (composition functions api)。API提供了依赖注入功能的新实现（`{ provide, inject }`）

```
import { provide, inject } from 'vue'

const ThemeSymbol = Symbol()

// 祖先组件节点provide
const Ancestor = {
  setup() {
    provide(ThemeSymbol, 'dark')
  }
}

// 后代组件可以inject获取
const Descendant = {
  setup() {
    const theme = inject(ThemeSymbol) // theme === 'dark
    return {
      theme
    }
  }
}
```

provide, inject不需要全局定义，仅在需要的组件族中使用，可控性更高。

![provide&inject](./vuex-replace.jpg)

## 支持Vue3组件库

---

> 适用于Vue3的UI库有哪些，各自迭代情况如何(参考在github上的star数、版本迭代频率等)

上Github上看，ElementUI已经接近一年没有高频率的commit了，最近大半年几乎没有commit。

![](./elementui.jpg)
[vue组件库——ElementUI全体成员跑路](https://www.cnblogs.com/han-1034683568/p/13540198.html)

vue3.0的环境下，使用ElementUI还要持观望态度，根据上述文章，可以选用的PC组件有：

1. [ant-design-vue](https://github.com/vueComponent/ant-design-vue/)

    * 个人开发者：[唐金洲](https://github.com/tangjinzhou)
    * Github Star 11.9k
    * 该组件库原型是大名鼎鼎的React组件库ant-design的Vue实现（复刻版），一开始是个人开发者，后来据查是获得了Vue官方的认可。UI样式风格比较接近ElementUI，文档风格也是很像。在不考虑未来稳定性的情况下，是ElementUI的一个比较好的替代品。


2. [Vuetify](https://github.com/vuetifyjs/vuetify)

    * Github Star 27.5k
    * 自2016年发布以来持续维护或者更新，Github Contributors显示每天都有commit。据传是Vue官方团队认证的最推荐组件库。团队成员基本来自国外的一众大佬。组件库丰富（比起ElementUI要少一些，但主要组件都有）。文档写的十分详细，支持简体中文，但是文档易用程度和阅读性感觉比起ElementUI要差点，或者说需要一个阶段来熟悉。

移动端组件库：

移动端组件库可选项比起PC端要多，大部分都是国内知名公司团队开发的。有赞商城团队的Vant，滴滴团队的Cube-UI和Mand-Mobile（后者主要针对金融业务），京东团队的NutUI。Github Star数量都是千位的数量级，公司移动端需求量不是特别大，可以按照实际需求再选择。

不过有一个一枝独秀的Star数上万（17.2k）的组件库Vux，作者是一个个人开发者，UI库是仿照和延伸微信官方样式设计，可按需使用。
