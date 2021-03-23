# 从babel说起

## @babel/parser

说这个库之前，我想大家一定很熟悉`JSON.stringify`（把对象编成字符串）和`JSON.parse`（把对象字符串编回对象本身）的作用。

那么`@babel/parser`这个库也起到类似的作用：将一个字符串类型的js代码变成一棵抽象语法树（AST）：

```js
import { parse } from '@babel/parser'
const code = `let a = 'let'; let b = 2`
const ast = parse(code, { sourceType: 'module' })
/*
ast = {
    program: {
        body: [{
            type: 'VariableDeclaration',
            kind: 'let',
            declarations: [{
                id: {
                    name: 'a',
                    ...
                },
                init: {
                    value: 'let',
                    ...
                },
                ...
            }, ...]
        }, ...],
        ...
    },
    ...
}
*/
```

以上代码中的注释中就是我们更有可能感兴趣的一些属性-值对。

`body`是代码主体，是一个代码块对象的数组。

每个代码块对象通过`type`区分代码块的类型，示例中是变量声明，因此其`type`为`'VariableDeclaration'`。

`kind`表示`let, const, var`声明。

声明的具体由`declarations`数组进行保存（考虑到js语言可以使用`,`同时声明多个变量，因此是由变量的数组进行存储）。

单个变量对象中值得关注的属性有两个，`···.id.name`是我们声明的变量名，`···.init.value`是我们声明变量的初始值。

获取到js代码转换成的AST，我们可以操作对象而不是使用正则操作字符串，这样对AST解析后，便能轻易地转译出不同版本的js代码

## @babel/traverse

babel都提供了字符串转译AST的工具，那么遍历AST自然也不需要我们手动来做。babel出品，AST的best match：`@babel/traverse`。

```js
import { parse } from '@babel/parser'
import traverse from '@babel/traverse'
const code = `let a = 'let'; let b = 2`
const ast = parse(code, { sourceType: 'module' })
traverse(ast, {
	enter: item => {
		if (item.node.type === 'VariableDeclaration') {
			if (item.node.kind === 'let') {
				item.node.kind = 'var'
			}
		}
	}
})
```

循环体item是对`ast.program.body[i]`进行一定程度上的改写，并不严格等于`ast.program.body[i]`，其属性更平面，可以让使用者更易操作。

现在来看看上述代码：`traverse`接受了两个参数：第一个是AST本身，第二个参数接受一个对象，对象中包含一个名叫`enter`的函数，该函数用于循环遍历AST，item为循环体。任何对item进行的属性改写，都会被直接回写AST，不需要返回值记录修改后的AST。

## @babel/generator

最后一步，babel送佛送到西，提供使用者类似`JSON.stringify`作用的库`@babel/generator`，想必大家能猜到其作用，就是将AST重译为js代码的字符串。使用时很简单：

```js
import { parse } from '@babel/parser'
import traverse from '@babel/traverse'
import generate from '@babel/generator'

const code = `let a = 'let'; let b = 2`
const ast = parse(code, { sourceType: 'module' })
traverse(ast, {
	enter: item => {
		if (item.node.type === 'VariableDeclaration') {
			if (item.node.kind === 'let') {
				item.node.kind = 'var'
			}
		}
	}
})
const newCode = generate(ast, {}, code)
console.log(newCode.code) // var a = 'let'; var b = 2;
```

> 可以亲自动手试试上述代码哦~

## 一步到位：ES6转译为ES5

在只是知道babel能够实现ES6 -> ES5的转译时，一度懵懂的觉得这一操作需要手写无数的代码和配置，来让babel正常工作。于是只好缩手缩脚的用脚手架开发一辈子。但实际上，babel的ES6 -> ES5转译其实也是脚手架哈（其实不是）。

babel封装好的使用方法很简单，我们直接来看一下代码：

```js
import { parse } from '@babel/parser'
import * as babel from '@babel/core'

const code = `let a = 'let'; let b = 2, d = 3; const c = 'c'`
const ast = parse(code, { sourceType: 'module' })
const result = babel.transformFromAstSync(ast, code, {
	presets: ['@babel/preset-env']
})
console.log(result.code)
/*
"use strict";

var a = 'let';
var b = 2,
    d = 3;
var c = 'c';
*/
```

和前面三节出现分歧的地方，就是直接使用了`@babel/core`提供的babel方法。有AST的情况下，使用`transformFromAstSync`方法，传入AST，js原代码字符串，和配置`presets`，很快啊！我们从返回的结果中，就能获取到转译完成的ES5代码啦~

> 今天只学了这么多，在学了在学了。。。
