---
title: css选择器
date: 2017-12-21 10:07:33
tags:
---
选择器主要是用来确定html的树形结构中的DOM元素节点。

我们把CSS选择器分开成四部分来介绍：

基本选择器| 多元素的组合选择器 | 属性选择器 |伪类选择器
---|---|---|---

## part1：基本选择器

| 选择器 | 含义|
|---|---|
| * | 通用元素选择器，匹配任何元素 |
| E | 标签选择器，匹配所有使用E标签的元素 |
| .className | 类选择器 |
| #ID | id选择器 |

补充：

| 选择器 | 含义|
|---|---|
| .className1.className2 | 多类选择器（ie6不支持） |

- 例子1：类选择器结合元素选择器

```
p.items {color: red;}   //匹配 p元素并且是其有一个类名叫“items”
```

- 例子2：多类选择器

```
.info.items {background:#ccc;}   //选择器只对元素中同时包含了"info"和"items"两个类才能起作用
```

## part2：多元素的组合选择器
| 选择器 | 含义|
|---|---|
| E,F | 多元素选择器，同时匹配所有E元素或F元素，E和F之间用逗号分隔 |
| E   F | 后代元素选择器，匹配所有属于E元素后代的F元素，E和F之间用空格分隔（所有后代元素） |
| E > F | 子元素选择器，匹配所有E元素的子元素F（仅仅是子元素） |
| E + F | 相邻兄弟元素选择器（相同的父元素），匹配所有紧随E元素之后的同级元素F |
| E ~ F | 通用兄弟元素选择器，选择某元素后面的所有兄弟元素（注意是兄弟元素） |

- demo：
```
<ul>
    <li>1</li>
    <li class="active">2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
    <li>9</li>
    <li>
        10
        <p>我是p元素</p>
    </li>
</ul>
```
- 例子：E + F

```
li + li {background: green;}  //选中2到10

.active + li {background: green;}  //选中3
```
- 例子：E ~ F

```
.active ~ li {color: red;} //选中3到10，p不会被选中
```

## part3：属性选择器

| 选择器 | 含义|
|---|---|
| E[att] | 匹配所有具有att属性的E元素，不考虑它的值。（注意：E在此处可以省略，比如"[cheacked]"。以下同。） |
| E[att=val] | 匹配所有att属性等于"val"的E元素 |
| E[att~=val] |匹配所有att属性具有多个空格分隔的值、其中一个值等于"val"的E元素|
| E[att^="val"] | 属性att的值以"val"开头的元素 |
| E[att$="val"] | 属性att的值以"val"结尾的元素 |
| E[att*="val"] | 属性att的值包含"val"字符串的元素 |
| E[att I=val] |特定属性选择器，匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以"val"开头的E元素，主要用于lang属性，比如"en"、"en-us"、"en-gb"等等|

- demo
```
<div class="demo">
    <a href="" class="" id="one" lang="zh-cn">1</a>
    <a href="http://git.linghit.com:666" class="" target="_blank">2</a>
    <a href="https://www.baidu.com" class="" title="hey man hello">3</a>
    <a href="" class="item4 info" lang="en-zh">4</a>
    <a href="" class="" id="five">5</a>
</div>
```

- 例子1：E[att] （单一属性的使用、多属性进行选择元素）

```
.demo a[id] {color:red;}  //选中1和5

.demo a[href][target] {color: yellow; }  //选中2，同时具备href与target属性
```

- 例子2：E[att=val]


```
.demo a[id="one"] {color:blue;}  // 选中1
/*也可以多个属性并写*/
.demo a[href="https://www.baidu.com/"][title] {color:#00ffc6;}   //选中3

```

> ps注意点：属性和属性值必须完全匹配


```
.demo a[class="item4"]{color: red};  //无法匹配到4
/*正确写法如下*/
.demo a[class="item4 info"]{color: red};
```

- 例子3：E[att~=val]，属性有多个值（空格隔开）是只需匹配到其中一个即可选中

```
//例子2中无法匹配到4,用下面这个就可以
.demo a[class~="item4"]{color: red};
```

- 例子4：E[attr^="value"]、E[att$="val"]、E[att*="val"]

```
.demo a[href^="http://"]{background:orange;} //选中2
.demo a[href^="https://"]{background:green;} //选中3

.demo a[href$="666"]{background:pink;}  //选中2

/*只要你所选择的属性，其属性值中有这个"value"值都将被选中*/
.demo a[title*="hello"]{background:black;}  //选中3
```

- 例子5：E[attr|="value"]，这个选择器会选择attr属性值等于value或以value-开头的所有元素

```
.demo a[lang|="zh"]{background:yellow;}  //选中1
```

> 比如说你页面中有很多图片，图片文件名都是以"figure-1","figure-2"这样的方式来命名的，那么使用这种选择器选中图片就很方便

## part4：伪类选择器

| 选择器 | 含义|
|---|---|
| E:lang(c) | 匹配lang属性等于c的E元素 |

（1） 动态伪类

| 选择器 | 含义|
|---|---|
| E:link | 匹配所有未被点击的链接 |
| E:visited | 匹配所有已被点击的链接 |
| E:active | 用于用户点击元素那一下的效果（正发生在点的那一下，松开鼠标左键此动作也就完成了）|
| E:hover | 匹配鼠标悬停其上的E元素 |
| E:focus | 匹配获得当前焦点的E元素 |

（2）UI元素状态伪类（主要用于Form元素）

| 选择器 | 含义|
|---|---|
| E:enabled | 匹配表单中激活的元素 |
| E:disabled | 匹配表单中禁用的元素 |
| E:checked | 匹配表单中被选中的radio（单选框）或checkbox（复选框）元素 |
| E::selection | 匹配用户当前选中的元素 |
-  "type="text"有enable和disabled两种状态，前者为可写状态后者为不可状态；
- type ="radio"和"type="checkbox""有"checked"和"unchecked"两种状态。

```
input[type="text"]:disabled { background:#ddd; }
```
（3）结构性伪类

| 选择器 | 含义|
|---|---|
| E:first-child | 选择某个元素的第一个子元素 |
| E:last-child | 选择某个元素的最后一个子元素 |
| E:nth-child() | 选择某个元素的一个或多个特定的子元素 |
| E:nth-last-child() | 选择某个元素的一个或多个特定的子元素，从这个元素的最后一个子元素开始算 |
| E:nth-of-type() | 选择指定的元素 |
| E:nth-last-of-type() | 选择指定的元素，从元素的最后一个开始计算 |
| E:first-of-type | 选择一个上级元素下的第一个同类子元素 |
| E:last-of-type | 选择一个上级元素的最后一个同类子元素 |
| E:only-child | 选择的元素是它的父元素的唯一一个了元素 |
| E:only-of-type | 选择一个元素是它的上级元素的唯一一个相同类型的子元素 |
| E:empty | 选择的元素里面没有任何内容 |


```
p:nth-child(3) { color:#f00; }
p:nth-child(odd) { color:#f00; } //奇数
p:nth-child(even) { color:#f00; } //偶数
p:nth-child(3n+0) { color:#f00; }
p:nth-child(3n) { color:#f00; }
tr:nth-child(2n+11) { background:#ff0; }
tr:nth-last-child(2) { background:#ff0; }
p:last-child { background:#ff0; }
p:only-child { background:#ff0; }
p:empty { background:#ff0; }
```
（4）伪元素

| 选择器 | 含义|
|---|---|
| E:first-line |	匹配E元素的第一行|
| E:first-letter |	匹配E元素的第一个字母|
| E:before|	在E元素之前插入生成的内容 |
| E:after |	在E元素之后插入生成的内容 |


```
p:first-line { font-weight:bold; color;#600; }

.preamble:first-letter { font-size:1.5em; font-weight:bold; }

.cbb:before { content:""; display:block; height:17px; width:18px; background:url(top.png) no-repeat 0 0; margin:0 0 0 -18px; }

a:link:after { content: " (" attr(href) ") "; }
```
伪类选择器内容有待继续更进...
























