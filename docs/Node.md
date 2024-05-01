

### 命令行

https://xiaoman.blog.csdn.net/article/details/132038424

初始化：

```sh
npm init -y
```

会生成一个`package.json`文件夹，`-y`表示参数使用默认值



运行scripts中的命令：

```sh
npm run <命令>
```



安装、卸载包：

```sh
npm i  # 安装package.json中记录的包
npm i vue@2.6.0
npm uni vue
```

开发时用到的依赖（如webpack）在devDependencies中

```sh
npm i 包名 --save-dev
npm i 包名 -D
```

查看npm配置

```sh
npm config list
```

配置镜像：

```sh
npm config set registry https://registry.npmmirror.com/
```

查看当前目录安装的包：

```sh
npm list
```

查看全局安装的包：

```sh
npm list -g
```



### nvm pnpm cnpm yarn

+ `nvm`：node版本管理工具

+ `pnpm`：更精简
+ `cnpm`：淘宝出品，适合国内
+ `yarn`：facebook出品

配置镜像：

```sh
npm config set registry https://registry.npmmirror.com
```

把`npm`改成`yarn`就可以配置`yarn`的镜像源





`nvm`

+ `nvm list`：查看已安装的node的版本
+ `nvm install 20.0.0` 安装node.js
+ `nvm use 20.0.0`：使用这个版本的nodejs。仅在当前终端有效







### 模块化

模块化规范遵循两套，一套`CommonJS`规范，另一套`esm`规范

#### CommonJS

node的默认规则

`package.json`中添加：

```json
"type":"commonjs",
```

引入模块（require）支持四种格式

+ 支持引入内置模块例如 http os fs child_process 等nodejs内置模块
+ 支持引入第三方模块express md5 koa 等
+ 支持引入自己编写的模块 ./ …/ 等
+ 支持引入addon C++扩展模块 .node文件

```js
const fs = require('node:fs');  // 导入核心模块
const express = require('express');  // 导入 node_modules 目录下的模块
const myModule = require('./myModule.js');  // 导入相对路径下的模块
const nodeModule = require('./myModule.node');  // 导入扩展模块
```

导出模块exports 和 module.exports

```js
module.exports = {
  hello: function() {
    console.log('Hello, world!');
  }
};
```

如果不想导出对象直接导出值

```js
module.exports = 123
```



#### esm

<a herf="##JavaScript">更多</a>

`package.json`中添加

```json
"type":"module"
```



导入：

+ 引入模块 `import` 必须写在头部

```js
import * as all from "./esm export.js"

console.log(all)
```

输出：

```
[Module: null prototype] {
  default: { success: 1, error: 0 },
  fn: [Function: fn],
  name: '111'
}
```





+ 动态导入模块：import静态加载不支持掺杂在逻辑中。如果想动态加载请，使用import函数模式

```js
if(true){
    import('./test.js').then()
}
```











导出

- 导出一个默认对象，default只能有一个，不可重复export default
- 导出定义而不是值

```js
export default {
    name: 'test',
}
```



```js
export const name = "1"
export function fn() { return 111 }

export const baseURL = 'http://hmajax.itheima.net'
export const getArraySum = arr => arr.reduce((sum, item) => sum += item, 0)

export default {
    success: 1,
    error: 0
}
```







Cjs 和 ESM 的区别

+ Cjs是基于运行时的同步加载，esm是基于编译时的异步加载
+ Cjs是可以修改值的，esm值并且不可修改（可读的）
+ Cjs不可以tree shaking，esm支持tree shaking
+ commonjs中顶层的this指向这个模块本身，而ES6中顶层this指向undefined



### fs

`fs.readFile(path[, option], callback)`读取指定文件中的内容，异步

1. path	必选参数，字符串，文件路径
2. option	可选参数，设置字符集
3. callback	必选参数，文件读取完成后的回调函数

+ 'a': 打开文件进行追加。 如果文件不存在，则创建该文件。
+ 'ax': 类似于 'a' 但如果路径存在则失败。
+ 'a+': 打开文件进行读取和追加。 如果文件不存在，则创建该文件。
+ 'ax+': 类似于 'a+' 但如果路径存在则失败。
+ 'as': 以同步模式打开文件进行追加。 如果文件不存在，则创建该文件。
+ 'as+': 以同步模式打开文件进行读取和追加。 如果文件不存在，则创建该文件。
+ 'r': 打开文件进行读取。 如果文件不存在，则会发生异常。
+ 'r+': 打开文件进行读写。 如果文件不存在，则会发生异常。
+ 'rs+': 以同步模式打开文件进行读写。 指示操作系统绕过本地文件系统缓存。
+ `'w'`: 打开文件进行写入。 创建（如果它不存在）或截断（如果它存在）该文件。
+ `'wx'`: 类似于 `'w'` 但如果路径存在则失败。
+ `'w+'`: 打开文件进行读写。 创建（如果它不存在）或截断（如果它存在）该文件。
+ `'wx+'`: 类似于 `'w+'` 但如果路径存在则失败。



```js
// 1. 导入 fs 模块，来操作文件
const fs = require('fs')

// 2. 调用 fs.readFile() 方法读取文件
//    参数1：读取文件的存放路径
//    参数2：读取文件时候采用的编码格式，一般默认指定 utf8
//    参数3：回调函数，拿到读取失败和成功的结果  err  dataStr
fs.readFile('./files/1.txt', 'utf8', function(err, dataStr) {
  
  // 2.1 打印失败的结果
  // 如果读取成功，则 err 的值为 null
  // 如果读取失败，则 err 的值为 错误对象，dataStr 的值为 undefined
  console.log(err)
  console.log('-------')
  
  // 2.2 打印成功的结果
  console.log(dataStr)
})

fs.readFile('./files/11.txt', 'utf8', function(err, dataStr) {
  if (err) {
    return console.log('读取文件失败！' + err.message)
  }
  console.log('读取文件成功！' + dataStr)
})
```

`fs.writeFile(path, data[, option], callback)`向指定的文件中写入内容

+ path	必选参数，字符串，文件路径
+ data	必选参数，写入的内容
+ option	可选参数，设置字符集，默认值是 utf8
+ callback	必选参数，文件写入完成后的回调函数
+ 注意：写入会覆盖原内容

```js
const fs = require('fs')

// 2. 调用 fs.writeFile() 方法，写入文件的内容
//    参数1：表示文件的存放路径
//    参数2：表示要写入的内容
//    参数3：回调函数
fs.writeFile('./files/3.txt', 'ok123', function(err) {
  // 2.1 如果文件写入成功，则 err 的值等于 null
  // 2.2 如果文件写入失败，则 err 的值等于一个 错误对象
  if (err) {
    return console.log('文件写入失败！' + err.message)
  }
  console.log('文件写入成功！')
})
```

### http

**参考蓝桥杯模拟赛一第八题**

```js
// 1. 导入 http 模块
const http = require("http");

// 2. 创建 web 服务器实例
const server = http.createServer();

// 3. 为服务器实例绑定 request 事件，监听客户端的请求
server.on("request", (req, res) => {
    // 定义一个字符串，包含中文的内容
    // req.url 是客户端请求的 URL 地址
    // req.method 是客户端请求的 method 类型
  const str = `您请求的 URL 地址是 ${req.url}，请求的 method 类型为 ${req.method}`;

  // 调用 res.setHeader() 方法，设置 Content-Type 响应头，解决中文乱码的问题
  res.setHeader("Content-Type", "text/html; charset=utf-8");

  // res.end() 将内容响应给客户端
  res.end(str);
});

// 4. 启动服务器
server.listen(8080, function () {  
  console.log('server running at http://127.0.0.1:8080')
})
```

+ `http.createServer() `:  就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务
+ `server.on("request", (req, res)=>{})`：为服务器绑定`request`事件，监听客户端的请求 
  + `req`其他事件：
  + `data`：当接收到请求体的一部分数据时触发。
  + `end`：当接收到所有请求体数据时触发。
    + 当接受完全部数据后，我们就可以开始构建响应`response`了
    + `res.write(xx.html)`可以把数据写回，而写这个函数可以多次调用，写回多次
    + `res.end([xxx])`这里也可以写，然后会结束响应
  + `close`：当底层连接（socket）被关闭时触发。
  + `error`：当接收和解析请求时出现错误时触发。
  + `aborted`：当请求被客户端中止时触发。
+ `serve.listen(8080, ()=>{})`：

服务端

```js
const http = require("http")
const url = require('node:url')

let server = http.createServer((req, res) => {
    const { pathname, query } = url.parse(req.url, true)
    if (req.method === "POST") {
        console.log(pathname);
        if (pathname === '/login') {
            let data = ""
            req.on('data', (chunk) => {
                data+=chunk
            })
            req.on('end', () => {
                res.setHeader('content-type', "application/json")
                res.statusCode = 200
                res.end(data)
            })
            
        } else {
            res.statusCode = 404
            res.end('404')
        }
    } else if (req.method === "GET") {
        if (pathname === "/api/list") {
            console.log(query);
            res.end('get')
        } else{
            res.statusCode = 404
            res.end('404')
        }
    }
})

server.listen(98, () => {
    console.log('server is running');
})
```

请求端（使用cost cilent插件）：

```js
GET http://localhost:98/api/listx?page=1&num=10 HTTP/1.1



# POST http://localhost:98/login HTTP/1.1

# Content-Type: application/json

# {
#     "name":"小满zs"
# }
```





解析请求

在 Node.js 的 HTTP 模块中，请求参数通常通过两种方式传递：查询字符串（query string）和请求体（request body）。处理这两种参数的方式略有不同。

1. 查询字符串参数：这些参数通常附加在 URL 的末尾，以 `?` 开始，参数之间以 `&` 分隔。例如，在 URL `http://example.com/?name=Alice&age=25` 中，`name` 和 `age` 就是查询字符串参数。在 Node.js 中，你可以使用内置的 `url` 模块来解析这些参数：

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const queryParameters = url.parse(req.url, true).query;
  console.log(queryParameters);  // 输出：{ name: 'Alice', age: '25' }
});

server.listen(3000);
```

2. 请求体参数：这些参数通常在 POST 或 PUT 请求中发送，包含在请求体中。在 Node.js 中，你需要手动读取和解析请求体。如果请求体是 JSON 格式的，你可以使用 `JSON.parse()` 函数来解析它：

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  let body = '';
  req.on('data', chunk => {
    body += chunk.toString(); // 将请求体数据转为字符串
  });
  req.on('end', () => {
    const requestBody = JSON.parse(body); // 解析 JSON 格式的请求体
    console.log(requestBody); // 输出：{ name: 'Alice', age: 25 }
  });
});

server.listen(3000);
```

注意，以上代码只处理了 JSON 格式的请求体。如果请求体是其他格式（例如 URL 编码的表单数据），你需要使用其他方法来解析。

另外，这些只是基本的处理方式。在实际开发中，你可能会使用 Express 这样的框架，它提供了更方便的方法来处理请求参数。





实例：
根据不同的 url 响应不同的 html 内容

1. 获取请求的 url地址

2. 设置默认的响应内容为 404 Not found

3. 判断用户请求的是否为 / 或 /index.html 首页

4. 判断用户请求的是否为 /about.html 关于页面

5. 设置 Content-Type 响应头，防止中文乱码

6. 使用 res.end() 把内容响应给客户端

```js
const http = require('http')
const server = http.createServer()

server.on('request', (req, res) => {
  // 1. 获取请求的 url 地址
  const url = req.url
  
  // 2. 设置默认的响应内容为 404 Not found
  let content = '<h1>404 Not found!</h1>'
  
  // 3. 判断用户请求的是否为 / 或 /index.html 首页
  // 4. 判断用户请求的是否为 /about.html 关于页面
  if (url === '/' || url === '/index.html') {
    content = '<h1>首页</h1>'
  } else if (url === '/about.html') {
    content = '<h1>关于页面</h1>'
  }
  
  // 5. 设置 Content-Type 响应头，防止中文乱码
  res.setHeader('Content-Type', 'text/html; charset=utf-8')
  
  // 6. 使用 res.end() 把内容响应给客户端
  res.end(content)
})

server.listen(80, () => {
  console.log('server running at http://127.0.0.1')
})
```



### express

服务端：

```js
let express = require('express')

let app = express()

// post想要使用req.body，需要这个中间件
app.use(express.json())

// get的参数在req.query
app.get('/get', (req, res) => {
    console.log(req.query);
    res.send('get')
})

// 监听/post上的post请求
// 动态参数：加冒号
// post的参数在req.body中
app.post('/post/:id', (req, res) => {
    console.log(req.body);
    res.send(`post${req.params.id}`)
})

app.listen(98, () => {
    console.log("server is running in 98");
})
```

请求（注意空行也要相同）：

```http
# GET http://localhost:98/get?page=1&num=10 HTTP/1.1

POST http://localhost:98/post/996 HTTP/1.1
Content-Type: application/json

{
    "name":"zs",
    "age":18
}
```



#### 模块化



```js
import express from 'express'

const router = express.Router() //路由模块


router.post('/login', (req, res) => {
    res.send('login')
})

router.post('/register', (req, res) => {
    res.send('register')
})


export default router
```



```js
import express from 'express';
import User from './src/user.js'
const app = express()
app.use(express.json())
app.use('/user', User)
app.get('/', (req, res) => {
    console.log(req.query)
    res.send('get')
})

app.get('/:id', (req, res) => {
    console.log(req.params)
    res.send('get id')
})

app.post('/create', (req, res) => {
    console.log(req.body)
    res.send('post')
})


app.listen(3000, () => console.log('Listening on port 3000'))
```

#### 中间件

```js
import log4js from 'log4js';

// 配置 log4js
log4js.configure({
  appenders: {
    out: {
      type: 'stdout', // 输出到控制台
      layout: {
        type: 'colored' // 使用带颜色的布局
      }
    },
    file: {
      type: 'file', // 输出到文件
      filename: './logs/server.log', // 指定日志文件路径和名称
    }
  },
  categories: {
    default: {
      appenders: ['out', 'file'], // 使用 out 和 file 输出器
      level: 'debug' // 设置日志级别为 debug
    }
  }
});

// 获取 logger
const logger = log4js.getLogger('default');

// 日志中间件
const loggerMiddleware = (req, res, next) => {
  logger.debug(`${req.method} ${req.url}`); // 记录请求方法和URL
  next();
};

export default loggerMiddleware;
```



使用：

```js
import express from 'express';
import User from './src/user.js'
import loggerMiddleware from './middleware/logger.js';
const app = express()
app.use(loggerMiddleware)
```





### readline

```js
const readline = require('readline');  
const rl = readline.createInterface({  
    input: process.stdin,  
    output: process.stdout  
});  
var inputArr = [];  
rl.on('line', function (input) {  
    inputArr = input.split(" ");  
    inputArr.forEach(function(item,index){  
        inputArr[index] = +item;// 转化为数字  
    });  
    // 下面就可以对数据进行处理......  
    console.log(inputArr);  
    inputArr = [];// 清空数组  
    rl.close();  
});  
  
rl.on('close', function() {  
    console.log('程序结束');  
    process.exit(0);  
});  
```



