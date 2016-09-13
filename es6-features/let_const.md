# Let 和 Const 用法

### Let
- 和 var 一样用来声明变量

    ```
        var a = 'let';
        let a = 'var';
    ``` 

- let 只在他声明的区块内有效 （作用域

    1. var
        
        ```
            var a = [];

            for (var i = 0; i < 10; i++) {
                a[i] = function () {
                    console.log(i);
                };
            }
            
            a[6]();
        
        ``` 

    1. let

        ```
        
            let a = [];

            for (let i = 0; i < 10; i++) {
                a[i] = function () {
                    console.log(i);
                };
            }
            
            a[6]();
        
        ``` 

    > 备注：使用后声明变量

    ```
        
        // 输出undefined
        console.log(foo);

        // 报错ReferenceError
        console.log(bar);

        var foo = 2;
        let bar = 2;

    ```

    > 以下是代码拆分

    ```

        // 使用var的情况
        var foo;

        console.log(foo);

        var foo = 2;

        // 使用let的情况
        console.log(bar);
        let bar = 2;

    ```


### const
- const

    ```
        const 在块级有效

        /**
         * 使用 const 声明的变量不能被改变
         * 我们在定义常量的时候都会用 const
         * 
         * 因为声明后值不可改变，所以我们声明的时候一定要给他赋值 
         */
        const Tabs = {
            Active: '0',
            InActive: '1'
        }

        const bar;
        bar = 0;

        /**
         * const 同样存在暂时性死区 所以也只能在声明的位置后面使用
         */

         console.log(v);
         const v = 'b';

         /**
          * const 声明的对象，可以添加属性
          * const 声明的数组，可以添加元素
          */

    ```
### 其他
- 重复声明

    ```

        function () {
            let a = 10;
            var a = 20;
        }

    ```

- 块级作用域

    > 不同的作用域中声明的变量

    ```
        function f1() {
            let n = 5;
            if (true) {
                let n = 10;
            }
            console.log(n);
        }
    ```

    > 广泛应用的立即执行匿名函数（IIFE）不再必要了。

    ```

        if (true) {
            (function () {
                console.log('----->');
            } ())
        }



        // ES5 中会输出 I am inside
        function f() {
            console.log('I am outside!');
        }

        (function () {
            if (false) {
                function f() {
                    console.log('I am inside!');
                }
            }
        }());

    ```



#### 备注
> 暂时性死区
>
> ES5规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。