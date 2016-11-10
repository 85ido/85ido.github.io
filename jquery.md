# jQuery

## 目录

- [背景](#background)

- [选择器和常用操作](#selector-tools)

    - [选择器](#selector)

        - [组合选择结果](#combine-result)

        - [性能优化](#performance)

            - [使用简单选择器](#simple-selector)

            - [缓存选择结果](#cache-selector-result)

    - [常用操作](#tools)

        - [添加和移除元素](#edit-element)

        - [修改元素内部](#element-content)

        - [获取元素属性](#element-attr)

        - [类和样式](#element-class)

- [事件绑定和处理](#events)

    - [事件代理](#events-delegate)

- [发送和处理请求](#ajax)

- [Question](#question)

---

<a name="background"></a>
## 背景

jQuery 最大的 **feature**，就是通过封装解决了各个平台操作DOM、事件监听和Ajax请求的差异。

文中代码是基于[jQuery 1.12.4](//cdn.bootcss.com/jquery/1.12.4/jquery.js)编写的。

<a name="selector-tools"></a>
## 选择器和常用操作

<a name="selector"></a>
### 选择器

jQuery的选择器强大而丰富，不仅支持全部CSS选择器，还支持部分扩展选择器。

```javascript
// jQuery 默认向 window 对象公开 $ 属性，所以在无冲突时 jQuery 和 $ 是等价的
$('div') // 选择所有 div 标签
$('#container') // 选择 Id 是 container 的元素
$('.btn') // 选择所有有 btn 类的元素
$('[data-todo]') // 选择所有包含 data-todo 属性的元素
$('all css selector') // 所有 css 选择器

// jquery 拓展的选择器
$(':button') // 选出所有 button 标签和 input[type='button']
$(':input') // 选择所有 input, textarea, select, button 元素
```

```javascript
// 选出百度首页的输入框
$('#kw')
```
![输出](images/jquery/id-selector.jpg)

注意：jQuery选择器返回的是Array Like（伪数组），大部分操作等同于操作返回结果，如果要逐个操作元素，可以使用 `$.each(array, callback)`。

*Array Like（伪数组）：就是有 length 属性的对象*

**$.each的回调函数参数非常反人类**

**$.each的回调函数参数非常反人类**

**$.each的回调函数参数非常反人类**

<a name="combine-result"></a>
#### 组合选择结果

可以通过 `,` 将多个选择器结果进行合并。

```javascript
$('#todo-list, .panel'); // 选择 Id 为 todo-list 的元素和所有有 panel 类的元素
```

<a name="performance"></a>
#### 性能优化

<a name="simple-selector"></a>
##### 使用简单选择器

尽可能的使用简单的 jQuery 选择器，避免不必要的层级嵌套。选择带有 Id 的元素，可以直接使用 Id 选择，不需要再使用其他选择器。

```javascript
$('#todo-list .new-todo-container #deep-dom-with-id');
// 上下选出的元素一致
$('#deep-dom-with-id')
```

<a name="cache-selector-result"></a>
##### 缓存选择结果

jQuery 选择器虽然强大，但强大带来的就是性能问题。有时候我们可能在一个函数中多次使用到某个元素，这时候如果每次用到时重新选择，并且选择器比较复杂的话，可能会造成较大的性能开销。如：

```javascript
// 获得 input 中的内容，并清空该 input，上下文略
var val = $('#todo-list .new-todo-container .new-todo').val();
// 处理 val，省略
// 清空 Todo
$('#todo-list .new-todo-container .new-todo').val('');
```

这时候应该将选择出的元素进行缓存，避免多次选择重复的元素。

```javascript
// jQuery 选择的元素，保存为变量一般是以$开头
var $newTodo = $('#todo-list .new-todo-container .new-todo');
var val = $newTodo.val();
$newTodo.val('');
```

<a name="tools"></a>
### 常用操作

通过 jQuery 选择出的元素，就可以对元素进行操作了，下面会讲解一些常用的 jQuery 操作，**这些操作必须要有选出的元素**。

<a name="edit-element"></a>
#### 添加和移除元素

`.append/prepend(...content)` 将 content 追加到元素内部

`.appendTo/prependTo(target)` append/prepend 两侧参数倒置

`.after/before(...content)` 将 content 添加到元素之后

`.insertAfter/insertBefore(target)` after/before 两侧参数倒置

`.remove` 将选中的元素从 DOM 中移除

content 和 target 可以是 html 字符串，Element 的实例，数组或 jQuery 对象

<a name="element-content"></a>
#### 修改元素内部

可以在获取到元素后修改元素内容，修改元素内容至少有三个方法。

`.html([str])` 获取（修改）元素内容，str 会被当作 html 内容

`.text([str])` 获取（修改）元素内容，str 会被当作纯文本，不会解析为 html 内容 

`.val([str])` 获取（修改）元素的值，一般用来向 input, textarea 元素赋值

<a name="element-attr"></a>
#### 获取元素属性

获取元素后可以对元素的属性进行操作

`.attr(name[, value])` 获取元素的 attribute，如果带有第二个参数，设置元素的 attribute 的值为 value

`.prop(name[, value])` 获取元素的 property，如果带有第二个参数，设置元素的 property 的值为 value

`.prop` 获取的是运行时的值，对于 `checked`, `selected` 属性，获取的是即时的值，使用 `.attr` 只会获取到定义在元素上的值

<a name="element-class"></a>
#### 类和样式

`.hasClass(className)` 判断元素上是否存在这个类

`.css(propertyName[, value])` 获取或设置指定样式

`.addClass(className)/.removeClass(className)` 在元素上添加/删除一个类

<a name="events"></a>
## 事件绑定和处理

可以把通过 jQuery 选出的元素绑定事件，使用 jQuery 绑定事件时，无需自己处理浏览器兼容性问题。

`$().on(eventName[, selector], callback);`

1. 在 jQuery 1.x 时代，绑定事件方法多种多样，有 `bind/unbind`， `delegate/undelegate`， `live/die`， `.事件名(callback)`， `on/off`，jQuery 已经打算废弃除了 `on/off` 以外的方法，所以在使用 jQuery 时，建议全部使用 **`on/off`** 去绑定和解除事件绑定。
0. 调用 `.on` 绑定的事件，每次调用都不会覆盖之前绑定的回调，而是添加一个事件回调。
0. 建议在 window 的 load 事件中进行事件绑定

```javascript
// 给所有带有 new-todo 类的元素绑定 click 事件
$('.new-todo').on('click', function() {
    alert('别摸我' + $(this).val()); // this 就是触发事件的元素
});
```

我们通过之前的追加元素的方法，向 body 中追加一个带有 new-todo 类的 input，再去点击追加的这个 input。

```javascript
$('body').append('<input class="new-todo" placeholder="动态添加的input">');
```

这时候我们发现，点击追加的 input，并没有弹出“别摸我”以其 input 的值。

<a name="events-delegate"></a>
### 事件代理

有时候，某些元素是在进行某些操作后才会出现在页面上（如：当添加一个 todo 后，todo 的列表才会有一个 li 元素显示 todo 内容）。这些元素在 window 的 load 事件中选择元素、绑定事件时还没有被创建，所以需要用事件代理的方式绑定。

事件代理一般是将动态元素的事件回调函数绑定到公共的父容器或者 body 上，在事件触发时其实是由被绑定的元素捕获的。

```javascript
// 给 #todo-list 中所有 .todo-item 元素绑定 click 事件
$('#todo-list').on('click', '.todo-item', function() {
    // special todo be clicked
});
```

<a name="ajax"></a>
## 发送和处理请求

jQuery 也提供了跨浏览器的和服务端交互的方法。

[`$.ajax`](http://api.jquery.com/jQuery.ajax/)

`$.ajax` 返回的对象是一个 [Promise Like](/es6-features/Promise-plus.md) 对象，可以通过 then 指定 resolved 和 rejected 的回调方法。

**不要使用 $.ajax settings 对象中的 success, error 回调处理 ajax 的响应**

**不要使用 $.ajax settings 对象中的 success, error 回调处理 ajax 的响应**

**不要使用 $.ajax settings 对象中的 success, error 回调处理 ajax 的响应**

```javascript
// jQuery 底层的 ajax 方法，在这个方法之上又封装了 $.get, $.post 和 $.getJSON
$.ajax(url[, settings])
    .then(
        function(data) {
            // success
            console.log(data);
        },
        function(error) {
            // error
            console.error(error);
        }
    );
```

<a name="question"></a>
## Question

1. 绑定的事件能否解绑，如果能，如何解绑指定的事件处理函数？

1. 如何对 jQuery 进行扩展？

1. 如果在请求完 `/todo-list` 后要请求 `/todo/1`，代码应该是什么样的？
