# JavaScript 代码规范

## 目录

1. 0

    1.1 规范等级

    1.2 标记

2. 代码风格

    2.1 文件

    2.2 缩进和空行

    2.3 换行

    2.4 语句

    2.5 命名

    2.6 注释

        2.6.1 单行注释

        2.6.2 文档注释

    2.7 逻辑块组织

3. 语言特性

    3.1 变量

    3.2 对象

    3.3 字符串

    3.4 数组

    3.5 流程控制
        
        3.5.1 条件

        3.5.2 类型检测

        3.5.3 循环

    3.6 函数

    3.7 类

    3.8 模块

4. jQuery

5. AngularJS

## 1. 0

### 1.1. 规范等级

每一条规范都有一个等级，说明该条规范的执行级别。规范等级有以下几种（按照等级排序）：

- **[强制]** 必须遵守的规范。

- **[建议]** 建议执行的规范，执行建议的规范会避免一些可能出现的问题。

### 1.2. 标记

规范中有一些斜体标识的标记，各标记有含义如下：

*A*：AngularJS 相关规范。

*E*：该规范会在 .editorConfig 中体现。

*6*：该条目仅在 ES6 环境下有效，其他环境可以忽略。

## 2. 代码风格

### 2.1. 文件

*这部分内容需要结合 Angular JS 部分一并阅读。*

**[强制]** 每个文件中只存在一个可以向外公开的部分。

**[强制]** 文件使用 camelCase 的方式命名。

**[强制]** 如果文件中存放的是类，文件与类名同名。

*①：对外公开的部分包括 Controller, Service, Directive, Component 等*

```javascript
// good
|-- js
|---- projectManage.js // angular controller
|---- datas
|------ User.js // export class
```

### 2.2. 缩进和空格

**[强制]** *E* 使用 `4` 个**空格**缩进。

```javascript
// good
function foo(options) {
    if (!options) {
        options = {};
    }
}

// bad
function baz(options) {
if (!options) {
    options = {};
}
}
```

**[强制]** `if`, `else`, `for`, `while`, `function`, `switch`, `try`, `catch`, `finally`, `function` 关键字后必须有一个空格。

**[强制]** 用作代码块起始的 `{` 前必须有一个空格。

**[强制]** 创建对象时，`:` 之后必须有一个空格，`:` 之前不允许有空格。

**[强制]** 函数声明和调用时，函数名和 `(` 之间不允许有空格。

**[强制]** `,` 和 `;` 之前不允许有空格；`,` 和 `;` 如果不位于行尾，之后必须有空格。

**[强制]** `()` 和 `[]` 内紧贴括号的部分不允许有空格。

**[强制]** 行尾不允许有多余的空格。

```javascript
// good
if (!options) {
    options = {};
}

// bad
if(options){
    options = {};
}

// good
var john = {
    name: 'John'
};

// bad
var mike = {
    name:'Mike',
    age : 5
};

// good
function foo() {
    // statement
}

// bad
function foo () {
    // statement
}

// good
for (var i = 0; i < arr.length; i++) {
    // loop
}

// bad
for (var i = 0;i < arr.length;i++) {
    // loop
}

// good
foo(10, 15);

// bad
foo(10,15);
```

**[强制]** 单行声明对象时，`{}` 内紧贴括号的部分必须有空格。

[解释] 如果对象的值比较简单，可以使用单行声明，建议都使用多行声明对象。

```javascript
// good
var obj = { enable: true };

// bad
var obj = {enable: true};
```

### 2.3. 换行

**[强制]** *E* 使用 `LF` 作为行结束符。

**[强制]** *E* 文件以一个空行结束。

**[强制]** 在每个语句结束后换行。

```javascript
// good
var age = 5;
var name = 'John';

// bad
var age = 5; var name = 'John';
```

**[强制]** *S* 每行不超过120个字符。

**[建议]** 不同的逻辑块之间使用换行隔开。

**[强制]** 在运算符处换行时，运算符必须在新行起始。

```javascript
// good
if (user.isLogin()
    && user.isEnable()
    && user.hasRole('manage.role')) {
    // Code
}

// bad
if (user.isLogin &&
    user.isEnable() &&
    user.hasRole('manage.role')) {
    // Code
}
```

**[强制]** 函数声明（表达式）、函数调用、对象（数组）创建，`for` 等场景换行时，`,` 和 `;` 不允许在新行起始。

**[强制]** `if`, `else`, `try`, `catch`, `finally` 代码块的 `}` 之后换行。

```javascript
// good
render(
    template,
    data1, data2, data3
);

// bad
render(
    template
    , data1, data2, data3
);
```

**[强制]** *6* 解构多个变量时，超出每行长度限制时，每个解构的变量单独一行，对象解构保持格式。

```javascript
// good
const { name, age } = john;
const {
    options: {
        xAxis,
        legend
    }
} = chartOptions;

// bad
const { config: { dataLoaded, theme } } = chartOptions;
```

**[建议]** 进行链式调用时，每个函数换行，确保每个链式调用的函数垂直对齐。

```javascript
// good
functionReturnsPromise()
    .then(data => {
        // do something with data
    })
    .catch(error => {
        // toastr for user, log error
    });

// bad
functionReturnsPromise().then(data => {
    // do something with data
}).catch(error => {
    // toastr for user, log error
})
```

**[建议]** 调用函数时，如果参数列表过长可以换行，每个或每个类型参数一行。

```javascript
// good
logToFile(
    functionReturnFilePaths(),
    date,
    author,
    triggerEvent
);

// bad
logToFile(functionReturnFilePaths(), data, author, triggerEvent);
```

### 2.4. 语句

**[强制]** 每个语句结束后必须添加 `;`。

**[强制]** `if`, `else`, `try`, `catch`, `finally` 语句不能省略块。

```javascript
// good
if (cod) {
    foo();
}

// bad
if (cod) foo()
if (cod)
    foo();
```

**[强制]** *6* 类声明不允许添加 `;`。

**[强制]** 函数定义, `export`, `export default` 不允许添加 `;`。

```javascript
// good
function foo() {
    // statement
}

export { foo }

// bad
function baz() {
    // statement
};
export { baz };
export default baz;
```

**[强制]** 立即执行的匿名函数（Immediately-Invoked Function Expression, IIFE）外添加 `()`，其他函数不允许添加 `()`。
    
### 2.5. 命名

**[强制]** `变量` 使用 `camelCase`。

**[强制]** `常量` 使用 `UPPER_CASE`（即全部字母大写，单词之间使用下划线）。

**[建议]** `常量` 放置在一个对象中，常量对象使用 `PascalCase`，每个 key 使用 `PascalCase`。

**[强制]** 函数，函数的参数使用 `camelCase`。

**[强制]** 类使用 `PascalCase`。

**[强制]** 类的方法、属性使用 `camelCase`。

**[强制]** 由多个单词组成的缩写词，在大小写上应该保持一致。

**[建议]** `boolean` 类型的变量，以 `has` 或 `is` 开头。

```javascript
// good
var age = 5;
var MAX_STUDENT_PER_CLASS = 10;
var Constants = {
    Env: 'production',
    MaxProject: 5
};
function doSomething(primary) {

}
function Person() {

}
Person.prototype.sayHello = function() {

};
function makeRequest(httpService) {
    
}
var isLoading = true;

// bad
var Age = 5;
var Max_Student_Per_Class = 10;
var constants = {
    ENV: 'production',
    MAX_PROJECT: 5
};
function DoSomething(Primary) {

}
function person() {
    // person is a constructor
}
person.prototype.SayHello = function() {

};
function makeRequest(hTTPService) {

}
var loading = false;
```
    
### 2.6. 注释

#### 2.6.1. 单行注释

**[强制]** 注释必须位于独立的行，注释在代码上方。

```javascript
// good
// options not null
if (options) {

}

// bad
if (options) { // options not null

}
```

**[建议]** 如果修改了非自己维护的代码，可以在改动的代码起始和结束位置注明改动日期和改动原因。

```javascript
// 2017-03-21 WheelJS 新增一个判断逻辑，避免返回意外的值
if (condition
    && anotherCondition) {
    
}
```

#### 2.6.2. 文档注释

**[建议]** 在可复用代码中编写文档注释。

解释：在 85ido 仓库中，使用基于 jsdoc, ngdoc 的文档格式，编写文档注释，可以直接生成 API 文档。

```javascript
/**
* 自动渲染模板，参数可以改变模板的行为。
*
* @param {object} options 模板配置项
* @param {boolean} [options.domDiff=false] 是否开启 virtual dom，默认 false
* @param {object} options.data 渲染模板使用的数据
* 
* @return Promise 渲染模板结束后会 resouilve，遇到错误时 reject
*/
function templateAutoRender(options) {

}
```

### 2.7. 逻辑块组织 *A*

每个逻辑单元的代码需要按照一定顺序组织。

**[强制]** 按照变量声明、初始化回调、事件监听、函数的顺序组织 Controller, Directive, Component。

```javascript
// good
angular.module('testModule', [])
    .controller('TestController', function($scope) {
        $scope.queryParams = {};
        $scope.userList = [];
        $scope.isLoading = false;

        this.$onInit = function() {
            $scope.query();
        };

        $scope.$watch('queryParams', function(newVal, oldVal) {
            // statement;
        }, true);

        $scope.$on('change.ktDict', function() {
            // event handler.
        });

        $scope.query = function() {
            // do query
        };
    });
```

## 3. 语言特性

### 3.1. 变量

**[强制]** 使用 `var` 声明变量，变量必须先声明后使用。

**[强制]** 每个 `var` 声明一个变量。

**[建议]** 变量声明在使用变量的代码附近。

解释：变量在真正使用前声明即可，在全局和函数开始时声明变量不利于查找。

```javascript
// good
function object2List(source) {
    var list = [];

    for (var key in source) {
        if (source.hasOwnProperty(key)) {
            var item = {
                key: key,
                value: source[key]
            };
            list.push(item);
        }
    }

    return list;
}

// bad
function object2List(source) {
    var list = [],
        key, item;

    for (key in source) {
        if (source.hasOwnProperty(key)) {
            item = {
                key: key,
                value: source[key]
            };
            list.push(item;)
        }
    }
}
```

**[建议]** *6* ES6 环境中，不使用 `var` 声明变量。

解释：ES6 引入了 `let` 和 `const` 声明变量，使用 `let` 和 `const` 可以避免作用域等问题，所有使用 `var` 的场景理论上都可以使用 `let` 代替。如果变量声明后不需要重新赋值，使用 `const`。

### 3.2. 对象

**[强制]** 使用对象字面量声明对象。

```javascript
// good
var john = {
    name: 'John',
    age: 27
};

// bad
var mike = new Object();
mike.name = 'Mike';
mike.age = 25;
```

**[强制]** 声明对象时，如果有一个属性名需要添加 `'`，则所有属性名都添加 `'`，否则所有属性都不添加 `'`。

```javascript
// good
var john = {
    'name': 'John',
    'age': 27,
    'class-name': 'Class 1'
};

// bad
var mike = {
    name: 'Mike',
    age: 25,
    'class-name': 'Class 3'
};
```

**[强制]** 声明对象时，如果有一个属性不指向同名变量，则所有属性都不使用缩写，否则所有属性使用缩写。

```javascript
// good
const john = {
    name,
    age,
    gender
};

const mike = {
    name: name,
    age: age,
    gender: 1
};

// bad
const mike = {
    name,
    age,
    gender: 1
};
```

**[强制]** 对象的最后一个属性值之后不允许添加 `,`。

解释：现阶段部分浏览器支持在对象最后一个属性值之后添加 `,`，但部分老旧浏览会报错。在找到解决方案前，不建议在最后添加 `,`。

**[强制]** 使用 `for in` 遍历对象的属性时，使用 `hasOwnProperty` 过滤非对象本身的属性。

解释：使用 `for in` 遍历对象时，原型上的属性名也会出现，使用 `hasOwnProperty` 可以判断，可以过滤掉原型上的属性。也可以使用 `Object.keys` 避免此问题。

```javascript
// good
function Person(name) {
    this.name = name;
}
Person.prototype.instanceFrom = 'Person';
var john = new Person('John');
for (var key in john) {
    if (john.hasOwnProperty(key)) {
        console.log(key);
    }
}

// good
Object.keys(john).forEach(key => console.log(key));

// bad
function Person(name) {
    this.name = name;
}
Person.prototype.instanceFrom = 'Person';
var john = new Person('John');
for (var key in john) {
    console.log(key);
}
```

**[强制]** 禁止覆盖内置对象的方法。

**[建议]** *6* 使用 `Object.keys` 遍历对象。

### 3.3. 字符串

**[强制]** 使用 `'` 声明字符串字面量，使用 `` ` `` 声明 ES6 模板字符串。

**[建议]** 使用数组拼接 HTML 字符串。

解释：使用数组拼接 HTML 字符串可以保持 HTML 的缩进结构。

```javascript
// good
var htmlContent = [
    '<ul>',
        '<li>',
            '<a>Tab 1</a>',
        '</li>',
        '<li>',
            '<a>Tab 2</a>',
        '</li>',
    '</ul>'
].join('');

// bad
var htmlContent = '<ul><li><a>Tab 1</a></li><li><a>Tab 2</a></li></ul>';
```

### 3.4. 数组

**[强制]** 使用数组字面量声明数组。

解释：使用 `new Array()` 时传入的参数会有歧义，使用字面量避免该问题。

```javascript
new Array(3, 4, 5)
// [3, 4, 5]

new Array(3)
// [undefined, undefined, undefined];
```

**[强制]** 使用 `Array.from` 将伪数组转换为真实数组。

**[建议]** 使用数组提供的 API 完成对数组的操作。

解释：ES6 引入的新的数组 API 相比 `for` 循环更加直接，使用数组 API 可以提高代码可读性，更加直观。

```javascript
// good
const selectedItem = list.filter(x => x._selected);

// bad
const selectedItem = [];
for (let i = 0; i < list.length; i++) {
    const item = list[i];
    if (item._selected) {
        selectedItem.push(item);
    }
}
```

### 3.5. 流程控制

**[建议]** 使用 `===` 比较相等，使用 `== null` 判断为 `null` 或 `undefined`。

解释：使用 `===` 避免隐式类型转换。

#### 3.5.1. 条件

**[建议]** 使用简洁的表达式。

解释：因为 JavaScript 的特性，在部分判断时可以使用简洁表达式，但部分场景使用简洁表达式可能会导致可读性降低。

**[建议]** 如果 `else` 之后不再有语句，可以删除 `else`。

```javascript
// good
function getName() {
    if (name) {
        return name;
    }
    return 'unnamed';
}

// bad
function getName() {
    if (name) {
        return name;
    }
    else {
        return 'unnamed';
    }
}
```

#### 3.5.2. 类型检测

**[强制]** 使用 `typeof` 检测基础数据类型，使用 `instanceof` 判断对象类型，检测 `null` 和 `undefined` 时使用 `== null`。

解释：检测一个变量是否为 `number`, `string` 等基础类型时，使用 `typeof`；检测是否为某个类型的实例时使用 `instanceof`。

**[强制]** 使用 `typeof` 检测类型时，必须使用 `===`，右侧的类型字符串不能出现意外值。

```javascript
var age = 5;
if (typeof age === 'number') {

}
```

#### 3.5.3. 循环

**[建议]** 在循环内部使用一个外部获取的值时，在循环外使用变量缓存。

```javascript
// good
var width = wrap.offsetWidth + 'px';
var len = elements.length;
for (var i = 0; i < len; i++) {
    var element = elements[i];
    element.style.width = width;
    // ......
}


// bad
for (var i = 0, len = elements.length; i < len; i++) {
    var element = elements[i];
    element.style.width = wrap.offsetWidth + 'px';
    // ......
}
```

### 3.6. 函数

**[强制]** *6* 箭头函数只有一个参数，并且不需要解构时，参数列表必须不添加括号。

**[强制]** *6* 箭头函数函数体只有一个表达式，且作为返回值时，必须省略 `{}` 和 `return`。

**[建议]** *6* 箭头函数函数体直接返回一个对象字面量时，可以省略 `return` 并使用 `()` 包裹。

```javascript
// good
const getServerUrl = env => {
    // Code
};

const selectedItems = displayList.filter(x => x.selected);

const getModalOptions = () => ({
    backdrop: 'static',
    keyboard: false
});

// bad
const sayHello = (name) => {
    console.log('Hello, my name is ' + name + '.');
};
```

**[强制]** 使用参数默认值代替条件判断默认值。

```javascript
// good
function open(url, fileName = url) {
    // Code
}

// bad
function open(url, fileName) {
    if (!fileName) {
        fileName = url;
    }
}
```

**[强制]** 使用 `...args` 代替 `arguments`。

解释：使用 `...args` 获取可变长度的参数。

```javascript
// good
function sum(...numbers) {
    return numbers.reduce((prevVal, x) => prevVal + x, 0);
}

// bad
function sum() {
    return Array.from(arguments).reduce((prevVal, x) => prevVal + x, 0);
}
```

**[建议]** 非数据输入参数使用 `options` 统一传递。

解释：一个函数的参数数量过多会导致参数顺序不好记忆，参数意义不直观等问题。使用 `options` 传递非数据类型，在方法传递时可以根据配置对象的 key 了解参数的意义，以及不需要记忆参数顺序。

### 3.7. 类

**[强制]** 使用 `class` 定义类。

**[强制]** 使用 `super` 访问父类。

### 3.8. 模块

**[强制]** 所有 `import` 写在模块开始处。

**[强制]** `export` 与内容定义放在一起。

```javascript
// good
export function foo() {

}
export const bar = 3

// bad
function foo() {

}
const bar = 3;

export { foo, bar }
```

### 3.10. 异步

**[建议]** 使用 `Promise` 代替 `callback`。

解释：`Promise` 可以在大多数场景下有效避免 `callback hell`。

**[建议]** 使用 `Bluebird` 的 `Promise.promisifyAll` 方法处理 Node 标准库。

解释：在使用较多 Node 标准库（含有 `callback` 参数）的方法时，可以使用 `Promise.promisifyAll` 方法对标准库命名空间进行处理，处理后可以获得标准库方法的 `Promise` 版本。

```javascript
import fs from 'fs';

// good
function readFileSafe(path) {
    return new Promise((resolve, reject) => {
        fs.stat(path, (err, status) => {
            if (err) {
                reject(err);
            }
            fs.readFile(path, (err, content) => {
                if (err) {
                    reject(err);
                }
                resolve(content);
            });
        });
    });
}

Promise.promifyAll(fs);
function readFileSafe(path) {
    fs.statAsync(path)
        .then(status => {
            return fs.readFileAsync(path);
        })
        .then(content => {
            console.log(content);
            return content;
        })
        .catch(err => {
            console.error(err);
        });
}

// bad
function readFileSafe(path) {
    fs.stat(path, (err, status) => {
        fs.readFile(path, (err, content) => {
            if (err) {
                throw err;
            }
            console.log(content);
        });
    });
}
```

**[强制]** 使用标准 `Promise` API。

解释：使用标准 `Promise` API，可以在运行环境都支持时，直接将 shim 去掉。在 Angular 1 环境中可以使用 `$q`。

**[强制]** 运行环境中没有 `Promise` 时，将 `Promise` 时实现 shim 到 `global` 中。

## jQuery

## AngularJS

## Node.js