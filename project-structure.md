# 项目结构

## 目录结构

### 前端

以 public/ 为主，下面的目录都是相对于 public/ 的目录

```
css 存放css，但是不会生成包含MD5的文件
fonts 存放字体，原样生成
img 存放图片，原样生成
js 存放编译后的js文件
    deps 存放依赖库
source 存放js源码，可以使用ES6语法，开发阶段会被编译到 js/ 中，发布时会被编译成包含MD5的文件
    components angular component
    directives angular directives
    filters angular filters
    modals 项目中Modal的Controller
    services angular services，命名建议使用camelCase
    validators custom validator，自定义验证器，一般在这里定义的是项目中通用的验证器
    module-name 根据模块可以拆分目录，目录中可以再存在 validators 目录，其他的不推荐
    main.js angular 主配置文件，如果添加了新的第三方库，则需要修改这个文件
    templates.js views/directives/ 中的模板文件生成的 $templateCache 代码，策略为 pager-tpl.jade -> pager.tpl，不要手动修改，如果需要修改，请去修改 views/directives/ 中对应的模板文件，发布时会直接按照该文件原样生成
```

### 前端Server

以下目录都是相对于项目根目录的

```
datas 请求后端接口方法的费鞥装
helpers jade 中使用的函数，建议一组相关联的函数在一个模块（文件）中
logs 按天生成的日志文件
middlewares express 中间件，在（某些）请求执行前后会被执行的 function
routes 路由文件，新增路由文件时需要在 app.js 配置请求的根路径
services 与业务无关的可复用代码，如：日志、请求代码的封装
views jade 文件，其中 directives/ 和 shared/ 有特殊处理，其他在发布时会原样输出
.babelrc babel 配置文件
.gitignore git 忽略的文件
.jshintrc/.eslintrc jshint/eslint 配置文件
app.js express 应用核心配置文件
workflow (将来会包含) gulp 子任务目录
gulpfile.babel.js gulp 任务核心（入口）文件
package.json node 项目描述文件，指定命令和依赖的库
README.md
```

## [gulp](https://github.com/lisposter/gulp-docs-zh-cn/blob/master/API.md)

gulp 提供的API十分简单，如果只是需要组合各种第三方插件，只需要了解各API的使用。

## [Babel](http://babeljs.io/)

babel 6不仅仅包含了将ES6转换成ES5的功能，还可以转换一些存在于ES7中的Features，也可以用来处理React(JSX)。

需要注意的是，如果想使用babel的6to5功能，必须引入 es2015 的 preset。

```json
{
    "presets": [ "es2015" ]
}
```
