# Array

## 数组基础操作

### 常用的操作

- 创建一个数组

    ```javascript

    /**
     * 创建数组的集中形式
     */

    // 创建一个实例，实例长度为0
    let array = new Array();

    // 创建一个默认带有长度的实例
    // 这种情况下push进去的东西会在默认长度后面追加
    let array = new Array(10);

    // Array 的属性
    array.length; 数组的长度

    ```

- 数组的取值方式

    ```javascript

    // 测试用的数组
    let cars = [ 'Saab', 'Volvo', 'BMW' ];
    let phones = [
        { name: 'RedMi', money: 1099 }, 
        { name: 'XiaoMi', money: 1099 },
        { name: 'Samsung', money: 2999 }
    ];

    // 数组取值方式
    cars[0];  // Saab

    phones[0]; // { name: 'RedMi' }
    phones[0].name; // RedMi

    /**
    * 注意：数组可以直接用 [下标] 直接取值，对象也可以通过 [] 方式来取值
    */

    let phone = { name: 'Nokia', money: 5999};

    // 对象的一种取值方式
    phone['name']; // Nokia

    // 上面说到的 phones[0].name 也就可以换成这么写
    phones[0]['name']; // RedMi

    // 遍历一个数组
    /**
     * 猜猜看下面的代码输出一致么?
     */
    
    for (let phone in phones) {
        console.log(phone);
    }

    for (let i = 0; i < phones.length; i++) {
        console.log(phones[i]);
    }

    ```

- 数组常用基本方法
    ```javascript
    /**
     * Array.join()
     * 把数组的元素放在一个字符串里面然后用特定的字符隔开
     * return 一个字符串
     */
     // 之前我们常用的是这样的
     let html = [ '<div>' ];

     html.push('<p> I am label P </p>');
     html.push('</div>');

     html.join(''); 

     /**
      * 后来这样写
      * 字符串模板
      */
        `
        <div>
            <p> I am label P </p>
        </div>
        `

    /**
     * Array.from()
     *
     * 方法可以将一个类数组对象或可遍历对象转换成真正的数组
     */

     Array.from(arrayLike[, mapFn[, thisArg]]);

    /**
     * 其他方法
     */

    // concat()	连接两个或更多的数组，并返回结果
    // of()  方法会将它的任意类型的多个参数放在一个数组里并返回
    // isArray()  判断某个值是否为数组，是返回 true，不是返回false
    // join()	把数组的所有元素放入一个字符串元素通过指定的分隔符进行分隔
    // pop()	删除并返回数组的最后一个元素
    // push()	向数组的末尾添加一个或更多元素，并返回新的长度
    // reverse()	颠倒数组中元素的顺序
    // shift()	删除并返回数组的第一个元素
    /**
     * slice 可以接受三个参数
     */
    // slice()	从某个已有的数组返回选定的元素
    // sort()	对数组的元素进行排序
    /**
     * 字幕排序的话， 数组内大小写，会分开排序
     */
    /**
     * splice 可以接受多个参数
     */
    // splice()	删除元素，并向数组添加新元素
    // toString()	把数组转换为字符串，并返回结果
    // unshift()	向数组的开头添加一个或更多元素，并返回新的长度

    ```

- 数组常用方法
    ```javascript

    /**
     * find()
     * 
     * 找到满足测试条件的item并返回， 找不到返回undefined
     */

    phones.find((item, index , phones) => {
        // item 为数组的每一项
        // index 为索引
        // phones 为原数组
        return item.name === 'RedMi';
    });

    /**
     * map()
     * 
     * 把数组中每项的元素放到一个新数组中返回
     * 
     * map 有时候 indexOf() 一起使用
     *
     */

    phones.map((item, index, phones) => {
        // item 为数组的每一项
        // index 为索引
        // phones 为原数组

        return item.name;
    });

    /**
     * filter()
     *
     * 用给定表达式测试数组每项，创建并返回一个包含测试通过的元素的数组，找不到 return 一个新数组
     * indexOf() 返回元素在数组中找到的第一个的索引，没有返回-1
     */

     let tmp = [ 'Xiaomi', 'RedMi' ];

     phones.filter((item, index, phones) => {
        // item 为数组的每一项
        // index 为索引
        // phones 为原数组

        return item.money === 1099;
        // return tmp.indexOf(item.name) > -1;
    });

    /**
     * Array.forEach()
     *
     * 数组的每一项都执行一次给定的函数
     */

     phones.forEach((item, index, phones) => {
        // item 为数组的每一项
        // index 为索引
        // phones 为原数组

        item.money + 1000;
    });

    /**
     * Array.some()()
     *
     * 测试数组中的某些元素是否通过了指定函数的测试
     */

     phones.some((item, index, phones) => {
        // item 为数组的每一项
        // index 为索引
        // phones 为原数组

        return item.money === 1000;
    });

    ```