# Promise后续

## 异步编程解决方案

1. 回调函数

    Node标准库常用的异步处理方案，这种方法不适合嵌套使用，会导致代码可读性降低。

    示例：判断文件是否存在，如果存在则读取文件内容
    ```javascript
    import fs from 'fs';
    const filePath = './editorconfig';
    fs.stat(filePath, (err, stats) => {
        if (err) {
            console.log(err);
            return;
        }

        fs.readFile(filePath, (err, file) => {
            if (err) {
                console.log(err);
            }
            console.log('File content:');
            console.log(file);
        });
    });
    ```

    又一个：先请求用户列表，再请求第一个用户的详细信息
    ```javascript
    $.ajax('user-list', {
        dataType: 'json',
        success(data) {
            if (data != null && data.length > 0) {
                const { id } = data[0];
                $.ajax(`user/${id}`, {
                    dataType: 'json',
                    success(user) {
                        // do something with first user
                    }
                });
            }
        }
    });
    ```

1. 事件

    jQuery和DOM事件中常用，也用于自定义事件系统（如：angularjs的scope之间通信使用的`$emit`, `$broadcast`, `$on`）。
    事件中也使用了回调函数，只不过事件中的回调函数可能会被多次触发。**事件和本文中提到的其他解决方案解决的问题不同。**

    示例：绑定DOM事件
    ```javascript
    window.addEventListener('load', () => {
        document.querySelector('#main').addEventListener('click', e => {
            console.log('container:#main be clicked(From DOM).');
        });
    });

    $(window).on('load', () => {
        $('#main').on('click', e => {
            console.log('container:#main be clicked(From jQuery).');
        });
    });
    ```

1. Promise

    **当前版本常用**，解决嵌套问题，介于兼容和最梦幻用法之间的用法。

    上面的示例：先请求用户列表，再请求第一个用户的详细信息
    ```javascript
    function getFirstUser() {
        return userService.list()
            .then(userList => {
                if (userList != null && userList.length > 0) {
                    return userService.get(userList[0].id);
                }
            })
            .then(firstUser => {
                return firstUser
            });
    }

    getFirstUser()
        .then(firstUser => {
            if (firstUser) {
                // do something with first user
            }
        })
    ```

1. 这里应该有个Generator函数，但是我不会啊

1. `async`和`await`

    **最梦幻用法**，存在于ES7中。

    高下立判的示例：先请求用户列表，再请求第一个用户的详细信息
    ```javascript
    async function getFirstUser() {
        const userList = await userService.list();
        if (userList != null && userList.length > 0) {
            const firstUser = await userService.get(userList[0].id);
            return firstUser;
        }
    }

    getFirstUser()
        .then(firstUser => {
            if (firstUser) {
                // do something with first user
            }
        });
    ```

1. 总结

    解决方案 | 优点 | 缺点 | 备注
    ------------ | ------------- | ------------ | -------------
    回调函数|-|可能会造成嵌套，异步之后的逻辑必须写在回调函数中|-
    事件|-|-|和其他异步方案解决不同的问题
    Promise|当前版本最佳，介于兼容和最梦幻用法之间，不容易造成嵌套|-|API的缺陷导致某些功能需要自己实现
    Generator|
    async和await|将异步操作写的像同步一样|兼容，即使使用babel也需要引入额外的js|-

## `Promise.all` 常见问题

有时在使用 `Promise.all` 时，需要进行的异步操作是在运行阶段确定的（如：只在条件符合的情况下才调用某些接口查询）。这样在 `Promise.all().then()` 中获取到的数组也是在运行阶段确定的，会导致无法通过之前的方式在 `Promise.all().then()` 中获取异步操作结果。

示例：
```javascript
const promises = [
    promise1
];
if (condition) {
    promises.push(promise3);
}
promises.push(promise2);

Promise.all(promises)
    .then(([ result1, result2, result3 ]) => {
        // 这里result2既有可能是promise3的结果，也有可能是promise2的结果
    });
```

此时的一种解决办法是，在每个 Promise 的 `then` 方法中获取结果，将处理结果之后返回的新 Promise 添加到 promises 中，将 `Promise.all` 作为 promises 中的所有 promise 都完成的标识。

```javascript
const viewData = {};
const promise = [
    promise1.then(result1 => viewData.result1 = result1)
];
if (codition) {
    promises.push(promise3.then(result3 => viewData.result3 = result3));
}

promises.push(promise2.then(result2 => viewData.result2 = result2));

Promise.all(promises)
    .then(() => {
        res.send(viewData);
    });
```
