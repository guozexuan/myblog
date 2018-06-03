---
title: chorme的Network面板简介
date: 2018-03-19 22:26:10
tags:
---

## 1.概述

Network面板可以记录页面上的网络请求的详情信息，从发起网页页面请求Request后分析HTTP请求后得到的各个请求资源信息（包括状态、资源类型、大小、所用时间、Request和Response等），可以根据这个进行网络性能优化。

## 2.Network 面板由五个窗格组成

1、Controls。使用这些选项可以控制 Network 面板的外观和功能。

2、Filters。 使用这些选项可以控制在 Requests Table 中显示哪些资源。提示：按住 Cmd (Mac) 或 Ctrl (Windows/Linux) 并点击过滤器可以同时选择多个过滤器。

3、Overview。 此图表显示了资源检索时间的时间线。如果您看到多条竖线堆叠在一起，则说明这些资源被同时检索。

4、Requests Table。 此表格列出了检索的每一个资源。 默认情况下，此表格按时间顺序排序，最早的资源在顶部。点击资源的名称可以显示更多信息。 提示：右键点击 Timeline 以外的任何一个表格标题可以添加或移除信息列。

5、Summary。 此窗格可以一目了然地展示请求总数、传输的数据量和加载时间。

示意图：

![image](https://raw.githubusercontent.com/guozexuan/images/master/network/panes.png)

### 默认情况下，Requests Table 会显示以下列。可以添加和移除列。

- Name。资源的名称。
- Status。HTTP 状态码。
- Type。已请求资源的 MIME 类型。
- Initiator。发起请求的对象或进程。值为以下选项之一：
    - Parser。Chrome 的 HTML 解析器发起请求。
    - Redirect。HTTP 重定向发起请求。
    - Script。脚本发起请求。
    - Other。某些其他进程或操作发起请求，例如用户通过链接或者在地址栏中输入网址导航到页面。
- Size。响应标头（通常为数百字节）加响应正文（由服务器提供）的组合大小。
- Time。从请求开始至在响应中接收到最终字节的总持续时间。
- Timeline。Timeline 列可以显示所有网络请求的可视瀑布。 点击此列的标题可以显示一个包含更多排序字段的菜单。

> HTTP 状态码

| 分类 | 分类描述 |
| :-- | :-- |
| 1** | 信息，服务器收到请求，需要请求者继续执行操作 |	
| 2** | 成功，操作被成功接收并处理 |
| 3** | 重定向，需要进一步的操作以完成请求 |
| 4** | 客户端错误，请求包含语法错误或无法完成请求 |
| 5** | 服务器错误，服务器在处理请求的过程中发生了错误 |

> 多用途Internet邮件扩展（MIME）类型 是一种标准化的方式来表示文档的性质和格式。通用结构：type/subtype，例如：text/html、image/jpeg、audio/ogg、video/mp4

## 3.记录网络活动

在 Network 面板打开时，DevTools 在默认情况下会记录所有网络活动。 要记录活动，只需在面板打开时重新加载页面，或者等待当前加载页面上的网络活动。

![image](https://raw.githubusercontent.com/guozexuan/images/master/network/btn.png)
您可以通过 record 按钮指示 DevTools 是否记录。 显示红色 (记录按钮打开) 表明 DevTools 正在记录。 显示灰色 (记录按钮关闭) 表明 DevTools 未在记录。 点击此按钮可以开始或停止记录，也可以按键盘快捷键 Cmd/Ctrl+e。

## 4.在记录期间捕捉屏幕截图
Network 面板可以在页面加载期间捕捉屏幕截图。此功能称为幻灯片。

点击摄影机图标可以启用幻灯片。图标为灰色时，幻灯片处于停用状态 (已停用幻灯片)。如果图标为蓝色，则说明已启用 (已启用幻灯片)。

重新加载页面可以捕捉屏幕截图。屏幕截图显示在概览上方。双击屏幕截图可查看放大版本。
![image](https://raw.githubusercontent.com/guozexuan/images/master/network/record.png)

## 5.查看 DOMContentLoaded 和 load 事件信息

>DOMContentLoaded事件会在页面上DOM完全加载并解析完毕之后触发，不会等待CSS、图片、子框架加载完成。 load事件会在页面上所有DOM、CSS、JS、图片完全加载完毕之后触发。

DOMContentLoaded事件在Overview上用一条蓝色竖线标记，并且在Summary以蓝色文字显示确切的时间。

load事件同样会在Overview和Requests Table上用一条红色竖线标记，在Summary也会以红色文字显示确切的时间。

![image](https://raw.githubusercontent.com/guozexuan/images/master/network/load.png)

## 6.查看单个资源的详细信息

点击资源名称（位于 Requests Table 的 Name 列下）可以查看与该资源有关的更多信息。下面四个标签最常见：

- Headers。与资源关联的 HTTP 标头。
- Preview。JSON、图像和文本资源的预览。
- Response。HTTP 响应数据（如果存在）。
- Cookies 显示资源HTTP的Request和Response过程中的Cookies信息。
- Timing。资源请求生命周期的精细分解。

#### ①查看 HTTP 标头

Headers 标签可以显示资源的请求网址、HTTP 方法以及响应状态代码。 此外，该标签还会列出 HTTP 响应和请求标头、它们的值以及任何查询字符串参数。

点击每一部分旁边的 view source 或 view parsed 链接，您能够以源格式或者解析格式查看响应标头、请求标头或者查询字符串参数。

![image](https://raw.githubusercontent.com/guozexuan/images/master/network/headers.png)

#### ②预览资源

点击 Preview 标签可以查看该资源的预览。Preview 标签可能显示一些有用的信息，也可能不显示，具体取决于所选择资源的类型。

#### ③查看资源HTTP的Response信息

在Response标签里面可根据选择的资源类型（JSON、图片、文本、JS、CSS）显示相应资源的Response响应内容。

#### ④查看 Cookie

如果选择的资源在Request和Response过程中存在Cookies信息，则Cookies标签会自动显示出来，在里面可以查看所有的Cookies信息。

>下面是 Cookie 表中每一列的说明：

- Name。Cookie 的名称。
- Value。Cookie 的值。
- Domain。Cookie 所属的域。
- Path。Cookie 来源的网址路径。
- Expires / Max-Age。Cookie 的 expires 或 max-age 属性的值。
- Size。Cookie 的大小（以字节为单位）。
- HTTP。指示 Cookie 应仅由浏览器在 HTTP 请求中设置，而无法通过 - JavaScript 访问。
- Secure。如果存在此属性，则指示 Cookie 应仅通过安全连接传输。

#### ⑤查看网络耗时

点击 Timing 标签可以查看单个资源请求生命周期的精细分解。
生命周期按照以下类别显示花费的时间：
- Queuing
- Stalled
- 如果适用：DNS lookup、initial connection、SSL handshake
- Request sent
- Waiting (TTFB)
- Content Download

> [官网参考](https://developers.google.com/web/tools/chrome-devtools/network-performance/resource-loading?hl=zh-cn)
> [中文参考文档](http://www.css88.com/doc/chrome-devtools/)