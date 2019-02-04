---
title: 浏览器对象模型 (BOM)
date: 2018-07-21 23:00:23
tags:
---

浏览器对象模型(Browser Object Model,BOM)：浏览器为js提供的对象集合。

# window
window对象表示浏览器中打开的窗口。windows对象是个全局对象，他不需要使用名称访问自己的属性和方法。
```
//下面的代码本质是相同的
alert("hello!");
 
window.alert("hello!");
```

如果文档包含框架（< frame > 或 < iframe > 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。

## window 尺寸

- window.innerHeight - 浏览器窗口的内部高度
- window.innerWidth - 浏览器窗口的内部宽度

> 内部宽高度:除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。

对于 Internet Explorer 8、7、6、5：
- document.documentElement.clientHeight / document.body.clientHeight
- document.documentElement.clientWidth / document.body.clientWidth

```
var w=window.innerWidth || document.documentElement.clientWidth ||document.body.clientWidth;

var h=window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
```

## 其他 Window 方法

- window.open() - 打开新窗口
- window.close() - 关闭当前窗口
- window.moveTo() - 移动当前窗口
- window.resizeTo() - 调整当前窗口的尺寸

## 对窗口操作

### 窗口引用

假设在一个网页中使用iframe或者frame标签, 那么在这个网页中就会有多个窗口
- window对象提供了window.top, window.parent, window.self三个属性表示顶层窗口, 父窗口和当前窗口. top, parent, self是对象, 也可以单独使用
- 我们的a标签, form标签等有个target属性, 表示在哪一个窗口打开, 这个属性有三个属性值, 分别于top, parent, self对象对应, 分别是_top, _parent, _self属性值.

```
<a href="/xxx' target="_parent">在父窗口中打开</a>
```

### iframe标签
这个标签可以在网页中嵌入一个窗口

- iframe标签遵守同源策略, 若父窗口和ifrmae来自不同域名则不能使用脚本直接通信, 要使用window.postMessage方法通信
- iframe嵌入窗口的window对象，有一个frameElement属性，返回它在父窗口中的DOM节点。对于那么非嵌入的窗口，该属性等于null

# navigator

navigator对象表示浏览器的信息，最常用的属性包括：

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的User-Agent字符串。

# screen

screen对象表示屏幕的信息，常用的属性有：

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如8、16、24。

# location

location对象表示当前页面的URL信息。location 对象是 Window 对象的一个部分，可通过 window.location 属性来访问。

可以用location.href获取URL各个部分的值:

```
//URL: https://zxcs.linghit.com/mllyuncheng/index.html?channel=zxcs&lap=0#email

location.protocol; // 'https'
location.host; // 'zxcs.linghit.com'
location.port; // 设置或返回当前 URL 的端口号。
location.pathname; // '/mllyuncheng/index.html'
location.search; // '?channel=zxcs&lap=0'
location.hash; // '#email'
```

## location 对象方法

- assign()  加载新的文档。
- reload()	重新加载当前文档。
- replace()	用新的文档替换当前文档。


# document

document对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，document对象就是整个DOM树的根节点。

document的title属性是从HTML文档中的< title >xxx</ title>读取的，但是可以动态改变：
```
document.title = '好好学习天天向上';
```

用document对象提供的getElementById()和getElementsByTagName()可以按ID获得一个DOM节点和按Tag名称获得一组DOM节点。

document对象还有一个cookie属性，可以获取当前页面的Cookie。
```
document.cookie;
```
由于JavaScript能读取到页面的Cookie,存在较大安全隐患，为了解决这个问题，服务器在设置Cookie时可以使用[httpOnly](https://www.cnblogs.com/zlhff/p/5477943.html)，设定了httpOnly的Cookie将不能被JavaScript读取。这个行为由浏览器实现，主流浏览器均支持httpOnly选项，IE从IE6 SP1开始支持。


# history

history对象保存了浏览器的历史记录，JavaScript可以调用history对象的back()或forward ()，相当于用户点击了浏览器的“后退”或“前进”按钮。

- back()	加载 history 列表中的前一个 URL。
- forward()	加载 history 列表中的下一个 URL。
- go()	加载 history 列表中的某个具体页面。

```
history.go(-1);     //后退1个页面
history.back();
 
history.go(1);       //前进1个页面
history.forward();
 
history.go(-2);     //后退2个页面
```

这个对象属于历史遗留对象，对于现代Web页面来说，由于大量使用AJAX和页面交互，简单粗暴地调用history.back()可能会让用户感到非常愤怒。任何情况，你都不应该使用history这个对象了。




