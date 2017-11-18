# Express 使用

Expressjs 是一个 Node.js 的 Web 应用 Framework。它为 Web 和移动应用提供了健壮的功能集合，可以应对常见 Web 开发中的场景，如：渲染页面、响应数据等。

本文档对应的 Express 版本是 `4.15.4`。

## 概念

### Router

*Router* 定义了应用的 URI，并决定这些 URI 是如何返回响应客户端请求的。

### Middleware

*Middleware* 是指从收到请求到执行 URI 对应的 *Router* 中间的处理流程，类似于 Java EE 中 *Filter*，Structs 中 *Interceptor* 的概念。

### 模板引擎

通过 Express 渲染的页面最终生成的是 HTML，但与 HTML 不同的是允许获取和显示一些服务端设置的值，循环一部分元素或某些元素根据逻辑决定是否渲染等。我们要使用一种模板引擎来完成这些生成 HTML 的工作（一般是 Jade，2016年以后叫 Pug）。

## Express-Generator

Express-Generator 可以帮助我们生成基础的 Express 应用的结构，我们的 Node 项目也是基于生成结果进行调整的。

### 使用 Express-Generator 生成项目结构

1. 安装 `express-generator`

    ```shell
    $ npm install express-generator -g
    ```

1. 以下的命令，在当前目录下创建一个名为 *myapp* 的目录，并会在 *myapp* 下创建 express 项目结构，使用 Pug 作为模板引擎。

    ```shell
    $ express --view=pug myapp
    ```

1. 安装依赖

    ```shell
    $ cd myapp
    $ npm install
    ```

1. 启动应用

    ```shell
    $ DEBUG=myapp:* npm start
    ```

## 应用结构

通过 Express-Generator 生成的项目结构如下，接下来对每部分进行说明。

```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── img(renamed from images)
│   ├── js(renamed from javascripts)
│   └── css(renamed from stylesheets)
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
├── views
|   ├── shared
|       ├── bundles.html
│       └── styles.html
|   ├── error.pug
|   ├── index.pug
|   └── layout.pug
├── .eslintrc.js
├── .editorconfig
└── .babelrc
```

- `app.js`：应用核心配置文件，包含默认中间件配置、*Router* 配置和全局错误处理配置。

- `bin/www`：默认启动文件，一般无需改动。应用监听的端口在该文件中配置。

- `package.json`：Node 应用核心配置文件，配置应用的依赖和信息。

- `public/`：公开资源目录，放在该目录下的资源默认可以直接被客户端访问。

- `routes/`：Router 目录。

- `views/`：视图目录，放在该目录下的视图需要有对应的 URI 渲染，客户端不能直接访问；文件扩展名由模板引擎决定（Pug -> .pug）。

- `views/shared`：引用页面基础静态资源的 html 存放路径，除了 &lt;script&gt; 和 &lt;style&gt; 以外一般还包含 &lt;!-- build --&gt; 注释。

- `views/layout.pug`：定义通用视图布局，视图部分组织方式请参考 Jade 培训文档。

- `.eslintrc.js`：ESLint 配置文件，无需改动。

- `.editorconfig`：editorconfig 配置文件，包含缩进配置，文件行结束符配置等。是所有**文本编辑器**自身或通过插件都可以识别的。无需改动。

- `.babelrc`：Babel 配置文件，无需改动。

## Router

1. 创建 *Router*

    在 routes/ 目录下创建一个新的 Router，每个 Router 文件都有的部分如下。

    ```javascript
    // routes/project.js
    import express from 'express';
    const router = express.Router();

    export default router;
    ```

1. 添加 URI

    ```javascript
    // When access this uri, 'views/project/dashboard.pug' will be render as html to client.
    router.get('/', function(req, res, next) {
        res.render('project/dashboard');
    });
    ```

    我们新建了一个 Router 并允许访问 `/`，但是目前还无法访问该 URI，原因是没有将路由配置到应用中。

1. 在 `app.js` 中配置

    ```javascript
    import project from './routes/project';
    // express generated code
    app.use('/projects', proejct);
    ```

    在 `app.js` 中配置之后，该 Router 中配置的 URI 才允许访问，完整 URI 是由 `app.use` 中配置的 URI 与 Router 中配置的 URI 拼接得到的。例如上例当中，访问 `/project/` 会返回对应的 `project/dashboard` 页面。

## API

在上述的例子中，我们使用了一部分 API 完成 Router 的创建、配置。现在对常用的 API 进行说明。

`express.Router()`

创建一个新的 Router 实例，可以在实例上配置 URI，Middlewares。

`router.use([path], function)`

配置 Router 的 Middleware(s)，如果有 `path` 参数则为指定 URI 配置。

`router.METHOD(URI[, MIDDLEWARES], HANDLER)`

用于创建一个 URI，其中：

- `METHOD`: 该 URI 请求的方式，可选值为当前 HTTP 协议所有允许的请求方式（get, post, patch, put, delete 等）。

- `URI`: 相对的 URI

    可以使用完整的字面量 URI

    可以使用正则表达式，匹配组从 `req.params` 中获取

    ```javascript
    router.get(/^\/commits\/(\w+)\/(\w+)$/, function(req, res, next) {
        const [ from, to ] = req.params;
        res.send(`diff between ${from} and ${to}`);
    });
    ```

    可以使用 URI Parameter，此时 `req.params` 是一个对象

    ```javascript
    router.get('/commit/:form/:to', function(req, res, next) {
        const { from, to } = req.params;
        res.send(`diff between ${from} and ${to}`);
    });
    ```

- `MIDDLEWARE`: 可选，当前 URI 使用的中间件，会按照顺序依次执行。

- `HANDLER`: 该 URI 的处理函数，每次请求都会执行该函数。参数签名一般为：`function(req, res, next) {}`，请求和响应对象会在执行 HANDLER 时被注入。

    `req`: 请求对象，可以通过 API 获取请求信息，如：body，headers等

    `res`: 响应对象，可以通过该对象将响应返回给 client

    `next`: 执行链中的下一个 handler，类似于 Java 中 `Filter#doChain` 操作

`app.use([path, ][function, ]function)`

在 `app.js` 中 `app = express()`，即为 express 的实例，该方法可以将 Middleware, Router 配置到全局中，`path` 参数为空时，所有请求都会执行配置的 Middleware，当 `path` 参数不为空时，则匹配 `path` 的请求会执行配置的 Middleware。

在 `app.js` 中配置的 Middleware，会按照配置顺序执行，Middleware 的函数签名与 Router handler 类似，

*TODO* 添加 req, res 常用方法文档
