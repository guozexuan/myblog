---
title: css变量
date: 2018-08-19 23:12:08
tags:
---


# 基本使用

CSS中原生的变量定义语法是：-- * ，变量使用语法是：var(-- * `   )，其中*表示我们的变量名称。变量名称没有限制（不能包含$，[，^，(，%等字符）。

- 声明一个局部变量：

```
element {
  --main-bg-color: brown;
  --1: #fff;
  --红色: red;
  --붉은: green;
  --_color1: green;
}
```

- 声明一个 全局 CSS 变量：

```
:root {
  --global-color: #666;
  --pane-padding: 5px 42px;
}
```
> :root 这个 CSS 伪类匹配文档树的根元素。对于 HTML 来说，:root 表示 <html> 元素，除了优先级更高之外，与 html 选择器相同。

- 使用

```
element {
  background-color: var(--main-bg-color);
}
```


# 作用域

- 同一个 CSS变量，可以在多个选择器内声明。读取的时候，优先级最高的声明生效。这与 CSS 的"层叠"规则是一致的。

- CSS 变量是支持继承的

- 通俗一点就是局部变量会在作用范围内覆盖全局变量。

- CSS变量并不支持 !important 声明。

```
:root { --color: blue; }
div { --color: pink; }
#purple { --color: purple; }
* { color: var(--color); }
    
<p>蓝色继承于根元素</p>
<div>粉色来自直接设置</div>
<div id='purple'>
    ID选择器权重更高，紫色
    <p>继承了紫色</p>
</div>
```

效果：

<font color=#0000ff> 蓝色继承于根元素 </font>

<font color=#f7b2be> 粉色来自直接设置 </font>

<font color=#f30ad6>ID选择器权重更高，紫色</font>

<font color=#f30ad6>继承了紫色</font>

总结：
- 变量的作用域就是它所在的选择器的有效范围。
- 覆盖规则由CSS选择器的权重决定


# CSS变量完整语法

var( [, ]? )，用中文表示就是：var( <自定义属性名> [, <默认值 ]? )，

1、变量未定义
```
p {
    color: var(--color, red)
}

<p>我是默认值颜色：红色</p>
```
效果：<font color=#f70404>变量未定义,我是默认值颜色：红色</font>


2、CSS变量不合法的缺省特性
```
body{
    color: blue;
}
p {
    --color: 20px;
    color: var(--color, red)
}

<p>变量不合法，颜色为蓝色</p>   
```
效果：<font color=#0000ff>变量不合法，颜色为蓝色</font>


# 变量类型

1、CSS变量值是数值，不能与数值单位直接连用。
```
p {
  --size: 20;   
  font-size: var(--size)px;
}
```
此处font-size:var(--size)px等同于font-size:20 px，注意,20后面有个空格。

或者用css3的calc：
```
font-size: calc(var(--size) * 1px);
```

2、css变量为字符串的组合使用

```
:root{
  --word:"this";
  --word-second:"is";
  --word-third:"CSS Variable";
}
 
div::before{
  content:var(--word)' 'var(--word-second)' 'var(--word-third);
}
```

[戳](https://codepen.io/zzzxuan/pen/vzNeWa)

# 响应式布局

可以在响应式布局的media命令里面声明变量，使得不同的屏幕宽度有不同的变量值。

```
css:

:root{
    --columns: 4;
    --margins: calc(24px / var(--columns));
    --space: calc(4px * var(--columns));
    --fontSize: calc(20px - 4 / var(--columns));
}
.box{
    background-color: aquamarine;
    width: 50%;
    min-width: 320px;
    margin: auto;
    overflow: hidden;
}
.cell{
    width: calc((100% - var(--margins) * var(--columns) * 2) / var(--columns));
    margin: var(--margins);
    background-color: #f0f3f9;
    float: left;
}
main {
    height: 150px;
    padding: var(--space);
    font-size: var(--fontSize);
}
@media screen and (max-width: 1200px) {
    .box {
        --columns: 3;
    }
}

@media screen and (max-width: 900px) {
    .box {
        --columns: 2;
    }
}

@media screen and (max-width: 600px) {
    .box {
        --columns: 1;
    }
}

html:

<div class="box">
    <div class="cell">
        <main>内容</main>
    </div>
    <div class="cell">
        <main>内容</main>
    </div>
    <div class="cell">
        <main>内容</main>
    </div>
    <div class="cell">
        <main>内容</main>
    </div>
    <div class="cell">
        <main>内容</main>
    </div>
    <div class="cell">
        <main>内容</main>
    </div>
</div>
```

[戳](https://codepen.io/zzzxuan/pen/oPNWva)


# JavaScript 操作

JavaScript 也可以检测浏览器是否支持 CSS 变量。

```
const isSupported =
  window.CSS &&
  window.CSS.supports &&
  window.CSS.supports('--a', 0);

if (isSupported) {
  /* supported */
} else {
  /* not supported */
}
```

```
// 设置变量
document.body.style.setProperty('--primary', '#7F583F');

// 读取变量
const rootStyles = getComputedStyle(document.documentElement);
const varValue = String(rootStyles.getPropertyValue('--primary')).trim();
// '#7F583F'

// 删除变量
document.body.style.removeProperty('--primary');
```

> JavaScript 可以将任意值存入样式表

```
const docStyle = document.documentElement.style;

document.addEventListener('mousemove', (e) => {
  docStyle.setProperty('--mouse-x', e.clientX);
  docStyle.setProperty('--mouse-y', e.clientY);
});
```

> getComputedStyle方法获取的是最终应用在元素上的所有CSS属性对象(即使没有CSS代码，也会把默认的祖宗八代都显示出来)，只读


# 兼容性

![image](https://mmc-forecast.oss-cn-shanghai.aliyuncs.com/images/467f9d12b7cc9d-2500x1060.png)

# 兼容性处理

对于不支持 CSS 变量的浏览器，可以采用下面的写法。

```
a {
  color: #7F583F;
  color: var(--primary);
}
```

也可以使用@support命令进行检测。
```
@supports ( (--a: 0)) {
  /* supported */
}

@supports ( not (--a: 0)) {
  /* not supported */
}
```


# 与传统 LESS 、SASS 等预处理器变量比较

1、CSS 变量的动态性，能在页面运行时更改，而传统预处理器变量编译后无法更改

2、CSS 变量能够继承，能够组合使用，具有作用域

3、配合 Javascript 使用，可以方便的从 JS 中读/写

# 有趣的用法

。。。

[参考链接](https://developers.google.com/web/updates/2016/02/css-variables-why-should-you-care)