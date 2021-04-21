# 朝花夕拾——JSONP到底是怎么写的

JSONP作为解决跨域问题的经典方案，我倒是很清楚它的实现原理。可是作为一个被现代前后分离式开发宠坏的人，默默的享受着CORS带来的便利，所以现在也是时候去细究一下前辈们的智慧成果了。

> 参考文献：[Huli——CORS 完全手冊（二）：如何解決 CORS 問題？](https://blog.huli.tw/2021/02/19/cors-guide-2/)

JSONP全称JSON with Padding。它的原理很简单，利用`script`标签不会被浏览器的同源策略针对，可以获取到请求的资源的全部内容。例如使用cdn引入一个`jquery.js`，那么我们就能在页面上使用`$`代指jquery。同样的，使用JSONP获取来的数据，也在`script`标签内等着我们调用；不但如此，还有骚操作是：我们在另一个`script`标签内定义一个函数，JOSNP获取到的数据中有调用该函数的操作，等到脚本执行到这里，就可以自动执行我们定义的函数来。

说了这么多定义和原理，可能还是不明白究竟要怎么来写，所以接下来看看代码吧！

## 第一层用法

一般我们使用数据可以通过

```html
<script>
    var user = {
        name: 'eleven',
        age: 25
    }
</script>
<script>
    console.log(JSON.stringify(user))
</script>
```

这时候来需求啦，user是登录的用户信息，不能再是静态数据，所以要这么改写


```html
<script src="http://localhost:8888/getUserInfo/1">
// 取得用户id为1的用户信息
// 同时后端返回数据为 var = user = { name: 'eleven', age: 25 }
</script>
<script>
    console.log(JSON.stringify(user))
</script>
```

这样就实现了一个简单的JSONP跨域。跨域是一个两端共有且更偏向后端解决的问题，只看一端永远不能搞清楚跨域，所以简单看一下后端代码：

```js
// 使用express.js作为后端框架
var express = require('express');
var app = express();

// 需要返回前端的数据
const users = {
    '1': {
        name: 'eleven',
        age: 25
    },
    '2': {
        name: 'twelve',
        age: 35
    }
}

// 简单使用一下表查询，返回正确的数据
app.get('/getUserInfo/:userId', function (req, res) {
  const userId = req.params.userId;
  res.end(`var user = ${JSON.stringify(users[userId])}`);
});

// 监听8888端口发来的请求
app.listen(8888, function () {
  console.log('Example app listening on port 8888!');
});
```

## 第二层用法

这时候又改变需求了，我们是管理者，需要多个用户的信息，不能只去获取id为1的用户了，那我们可以这么写

```html
<body>
  <button onclick="getUser(1)">user1</button>
  <button onclick="getUser(2)">user2</button>
    <script>
    function getUser(userId) {
        // 新增script元素
        const script = document.createElement('script')

        // 为新增的script标签加src
        script.src = 'http://localhost:8888/getUserInfo/' + userId

        // 插入到body中
        document.body.appendChild(script)

        // 打印结果
        console.log(JSON.stringify(user))
    }
    </script>
</body>
```

这种方式的调用在原理上可以理顺，但实际使用时，通过接口请求数据慢了一步，在执行`console.log(JSON.stringify(user))`之后才真正获取到数据，那这段逻辑就会报错。

因此，我们可以使用回调函数来异步打印获取结果，由此引出第三层用法。

## 第三层用法

首先我们需要一个回调函数来执行打印接口数据的操作，`function uUserInfo`：

```html
<body>
    <button onclick="getUser(1)">user1</button>
    <button onclick="getUser(2)">user2</button>
    <script>
        function uUserInfo(info) {
            // 打印结果
            console.log(JSON.stringify(info))
        }
        function getUser(userId) {
            const script = document.createElement('script')
            script.src = 'http://localhost:8888/getUserInfo/' + userId
            document.body.appendChild(script)
        }
    </script>
</body>
```

光是前端可没法知道哪一刻才是获取到数据的时候，因此执行回调的事情就要交给后端来完成：

```js
var express = require('express');
var app = express();

// 需要返回前端的数据
const users = {
    '1': {
        name: 'eleven',
        age: 25
    },
    '2': {
        name: 'twelve',
        age: 35
    }
}

// 简单使用一下表查询，返回正确的数据
app.get('/getUserInfo/:userId', function (req, res) {
  const userId = req.params.userId;

  // 注意这里！返回了函数调用的字符串，当script节点被插入body后，就会自动执行这一调用，同时把结果作为传参
  res.end(`uUserInfo(${JSON.stringify(users[userId])})`);
});

// 监听8888端口发来的请求
app.listen(8888, function () {
  console.log('Example app listening on port 8888!');
});
```

这个方案看起来已经比较完美了，除了一点，这个`uUserInfo`是什么鬼，拼错单词了吧！新入职的强迫症前端想要改了这个小bug，于是去通知后端一起做修改，后端表示：很麻烦，如果同意你这次，下次还想换名字怎么办，请一劳永逸的解决命名问题。

好的，我们来看下一层。

## 最后一层用法


既然函数定义由前端完成，那么函数命名由前端通知给后端不就好了？于是：

```html
<body>
    <button onclick="getUser(1)">user1</button>
    <button onclick="getUser(2)">user2</button>
    <script>
        function useUserInfo(info) {
            // 打印结果
            console.log(JSON.stringify(info))
        }
        function getUser(userId) {
            const script = document.createElement('script')
            script.src = `http://localhost:8888/getUserInfo/${userId}?cb=useUserInfo`
            document.body.appendChild(script)
        }
    </script>
</body>
```

通过url传递函数名给后端，后端只要写传递的参数就好：

```js
var express = require('express');
var app = express();

// 需要返回前端的数据
const users = {
    '1': {
        name: 'eleven',
        age: 25
    },
    '2': {
        name: 'twelve',
        age: 35
    }
}

// 简单使用一下表查询，返回正确的数据
app.get('/getUserInfo/:userId', function (req, res) {
  const userId = req.params.userId;
  const fnName = req.query.cb;

  // 注意这里！返回了函数调用的字符串，当script节点被插入body后，就会自动执行这一调用，同时把结果作为传参
  res.end(`${fnName}(${JSON.stringify(users[userId])})`);
});

// 监听8888端口发来的请求
app.listen(8888, function () {
  console.log('Example app listening on port 8888!');
});
```

没问题了！至此我们就把JSONP中的原理全部付诸实际了。接下来进行技术总结：

JSONP的出现是因为浏览器的同源策略，ajax获取数据时，会被浏览器挡住跨域的响应。而html中的`img script`标签不受同源策略的限制，因此可以用`script`获取不同域下的资源。获取到的内容会像其他通过cdn获取的js文件一样，把字符串直接插入`script`标签内变为可执行的代码。这样一来，就可以返回函数调用字符串，把结果作为传参，插入`script`内，自动执行在其他`script`标签内部定义的函数，完成一次JSONP操作。

JSONP的上限受制于`script`标签的上限：只能发送GET类型的请求。
