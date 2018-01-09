# 检查 Flash 和请求使用 Flash

## 0. 背景

在最近的 Chrome, Firefox, Edge 更新后，访问网站时默认不开启 Flash。这会导致使用 Flash 作为视频播放器和基于 Flash 的文件上传功能不可用。

要在用户体验上变得顺滑，大体上可以从检测和请求权限上下手。

## 1. 检测

检测浏览器是否启用了 Flash。这里可以借用 Yahoo 多年不维护的一个仓库 [http://featureblend.com/javascript-flash-detection-library.html](http://featureblend.com/javascript-flash-detection-library.html)。

在这个需求中，我们只需要用到的一个 `installed` 属性就可以判断 Flash 是否被启用了。这个库还可以拿到正在使用的 Flash 的版本号，具体可以参考文档。

```javascript
if (FlashDetect.installed) {
    // do something with flash
}
else {
    // show notification or auto request flash permission
}
```

## 2. 请求权限

虽然在 Chrome（Edge) 中，请求 Flash 权限与请求 Geolocation, microphone 等权限交互一致，但是通过 PermissionAPI 并不能请求 Flash 权限，Flash 权限是通过一种很奇妙的方式请求的。

简单的说：当在页面中通过 `<a>`, `<iframe>` 这类标签请求 `https://get.adobe.com/flashplayer/` 时，Chrome, Edge 会捕获这个请求，不进行跳转并在地址栏左上角弹出提示询问用户是否允许该网站请求 Flash。

参考：[Chromium Flash Roadmap](https://www.chromium.org/flash-roadmap#TOC-Developer-Recommendations)

扩展阅读：[Permissions.query() - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Permissions/query)

所以我们可以通过浏览器的这个特性，配合上述的检查来优化启用 Flash 的用户体验。

## 3. 应用

在上传头像页中，上传按钮是基于 Flash 的，兼容老版本浏览器的，但是在 Flash 未开启的情况下，插件初始化时会静默失败，不会正确初始化。所以我们要对前置流程进行优化，优化内容如下：

1. 在用户的浏览器不允许 Flash 运行时（Chrome 默认为“询问”，但实际也不能运行），隐藏选择图片按钮并提示用户。

    效果：

    ![Flash 未启用状态](images/flash-detect-1.jpg)

1. 在用户的浏览器不允许 Flash 运行时，自动请求 Flash 运行权限。

    ![请求 Flash 权限](images/flash-detect-2.jpg)

    根据 “请求权限” 部分我们可以知道，当请求特定 Url 时会被 Chrome, Edge 捕获并询问用户，所以我们可以在检测到 Flash 未启用时生成一个 `<iframe>` 并将 src 指向特定 Url。

```javascript
function requestFlashPermission() {
    var $iframe = $('<iframe>');
    $iframe.addClass('hide');
    $iframe.attr('src', 'https://get.adobe.com/flashplayer/');
    $('body').append($iframe);
}

if (FlashDetect.installed) {
    // display upload button
}
else {
    // display flash disabled tips
    $('.js-flash-disabled').removeClass('hide');
    // auto request flash permission
    requestFlashPermission();
}
```
