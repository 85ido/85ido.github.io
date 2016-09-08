# Arrow function

## 最简单的箭头函数

```javascript
var func = x => x;
```

等同于

```javascript
var func = function(x) {
    return x;
};
```

## 箭头函数介绍

1. 参数

    ```javascript
    // 没有参数
    var f = () => console.log('hello world');

    // 有一个参数
    var f = (x) => console.log('x = ' + x);
    var f = x => console.log('x = ' + x);

    // 有多个参数
    var f = (x, y) => console.log('x = ' + x + ',y = ' + y);
    ```

    当箭头函数只有1个参数时，参数部分可以不使用括号，否则参数部分必须有括号

1. 返回值

    ```javascript
    // 只有一条语句时，执行结果作为返回值
    var f = () => Math.min(1, 2);
    // output: 1

    // 多于一条语句时，使用return返回值
    var f = () => {
        console.log('Math.min(1, 2)');
        console.log(Math.min(1, 2));
        return 'hello world';
    };
    ```

1. `this`和构造函数

    箭头函数中的`this`是在定义时确定的，而不是运行时确定的，箭头函数创建的作用域没有`this`。
    因为没有`this`，所以箭头函数也不能用作构造函数，不能使用.call, .apply, .bind改变this指向，这就是**箭头函数的`this`是在定义时确定的，而不是运行时确定的。**

    ```javascript
    var obj = {
        persons: ['John', 'Mike', 'Ana'],
        init: function() {
            this.persons.forEach((x) => this.say(x));
        },
        init2: function() {
            this.persons.forEach(function(x) { this.say(x) });
        },
        init3: () => {
            this.persons.forEach(x => this.say(x));
        },
        say: function(name) {
            console.log(name + ' say hello to you');
        }
    };
    ```

    在angular中，当我们需要使用controller#$onInit时，就不能使用箭头函数定义一个controller。

    ```javascript
    angular.module('myApp')
        .controller('TestCtrl', ($scope) => {
            'ngInject';

            this.$onInit = () => {
                console.log('TestCtrl#$onInit');
            };
        });

    angular.module('myApp')
        .controller('TestCtrl2', function($scope) {
            'ngInject';

            this.$onInit = () => {
                console.log('TestCtrl2#$onInit');
            };
        });
    ```

1. arguments

    箭头函数中不存在arguments，如果要取到所有参数，可以使用rest参数

    ```javascript
    // 报出 ReferenceError
    var say = () => {
        console.log(arguments);
    };
    // 可以打印出传递的参数们
    var say = (...args) => {
        console.log(args);
    };
    ```

## 箭头函数的使用场景

1. 简化回调函数

    ```javascript
    var names = ['Mike', 'John', 'Ana'];
    names.forEach(function(name) {
        console.log(name)
    });
    names.forEach(name => console.log(name));

    $http.get('/path/to/list')
        .then(function(resp) {
            // do something with resp...
        });

    $http.get('/path/to/list')
        .then(resp => {
            // do something with resp...
        });
    ```

1. 在不需要使用`this`的地方，代替`function`

    除了需要使用`this`的函数，几乎所有函数都可以使用箭头函数代替
