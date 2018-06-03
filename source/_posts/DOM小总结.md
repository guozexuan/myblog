---
title: DOM小总结
date: 2017-11-06 09:18:45
tags:
---
# 1、基本概念
## 1.1、DOM
DOM是JavaScript操作网页的接口，全称为“文档对象模型”（Document Object Model）。它的作用是将网页转为一个JavaScript对象，从而可以用脚本进行各种操作（比如增删内容）。

浏览器会根据DOM模型，将结构化文档（比如HTML和XML）解析成一系列的节点，再由这些节点组成一个树状结构（DOM Tree）。

## 1.2、节点

DOM的最小组成单位叫做节点（node）。文档的树形结构（DOM树），就是由各种不同类型的节点组成。每个节点可以看作是文档树的一片叶子。
（==看图 go==）

> 根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：

- 整个文档是一个文档节点
- 每个 HTML 元素是元素节点
- HTML 元素内的文本是文本节点
- 每个 HTML 属性是属性节点
- 注释是注释节点

## 1.3、DOM节点信息

### （1）节点类型（nodeType）、节点名称（nodeName）

 nodeType属性返回节点类型的常数值；nodeName返回当前节点的节点名称

类型 | nodeType | nodeName
---|---| -
 Node.ELEMENT_NODE | 1 | 大写的HTML元素名（标签）
 Node.TEXT_NODE | 3 | #text
 Node.PROCESSING_INSTRUCTION_NODE | 7  | 一个用于XML文档的 ProcessingInstruction ，例如 <?xml-stylesheet ... ?> 声明。
 Node.COMMENT_NODE | 8 | #comment （注释节点）
 Node.DOCUMENT_NODE | 9  | #document（文档节点）
 Node.DOCUMENT_TYPE_NODE |  10 | 描述文档类型的 DocumentType 节点。例如 <!DOCTYPE html>  就是用于 HTML5 的。等同于DocumentType.name
 Node.DOCUMENT_FRAGMENT_NODE |  11 | #document-fragment，一个 DocumentFragment 节点（文档的片段）

**备注：** 另外还有五种节点类型已弃用

参考： https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType



> 下表列出了所有类型的节点的nodeName属性的值. （看看就好。。）

接口 |	nodeName属性值
---|---
Attr	|等同于 Attr.name 属性的值
CDATASection|	"#cdata-section"
Comment	 | "#comment"
Document | 	"#document"
DocumentFragment | 	"#document-fragment"
DocumentType | 	等同于 DocumentType.name 属性的值
Element	 | 等同于 Element.tagName 属性的值
Entity | 	实体名称
EntityReference | 	实体引用名称
Notation | 	Notation名称
ProcessingInstruction | 等同于 ProcessingInstruction.target 属性的值
text | 	"#text"

**栗子：**

使用nodeType属性确定一个节点的类型，比较方便。

```
document.querySelector('a').nodeType === 1
// true

document.querySelector('a').nodeType === Node.ELEMENT_NODE
// true
```


ps：querySelector() 方法返回文档中匹配指定 CSS 选择器的一个元素。

### （2）节点值（nodeValue）
Node.nodeValue属性返回一个字符串，表示当前节点本身的文本值，该属性可读写。

由于只有Text节点、Comment节点、XML文档的CDATA节点有文本值，因此只有这三类节点的nodeValue可以返回结果，其他类型的节点一律返回null。同样的，也只有这三类节点可以设置nodeValue属性的值。对于那些返回null的节点，设置nodeValue属性是无效的。



## 1.4 、节点关系  

- parentNode    父节点
- childNodes    子节点
- firstChild    父节点里的第一个子节点
- lastChild     父节点里的最后一个子节点
- nextSibling    与子节点并列的下一个兄弟节点
- previousSibling   与子节点并列的上一个兄弟节点

```
<html>
  <head>
    <title>DOM</title>
  </head>
  <body>
    <h1>我的父节点是谁？</h1>
    <p>Hello world!</p>
  </body>
</html>
```

> （1）Node.parentNode

parentNode属性返回当前节点的父节点。对于一个节点来说，它的父节点只可能是三种类型：元素(Element )节点、文档(Document )节点、文档碎片(DocumentFragment)节点。


> （2）Node.childNodes

childNodes属性返回一个NodeList集合，成员包括当前节点的所有子节点。注意，除了HTML元素节点，该属性返回的还包括Text节点和Comment节点。如果当前节点不包括任何子节点，则返回一个空的NodeList集合。由于NodeList对象是一个动态集合，一旦子节点发生变化，立刻会反映在返回结果之中。

```
var ulElementChildNodes = document.querySelector('ul').childNodes;
```

**ps**：
1. NodeList 对象代表一个有顺序的节点列表。可通过节点列表中的节点索引号来访问列表中的节点(索引号由0开始)；也可以用 length 属性来确定指定类名的元素个数。

```
//查看文档中有多少个样式 class="example" 的元素
document.getElementsByClassName("example").length;
```

> （3）Node.firstChild，Node.lastChild
firstChild属性返回当前节点的第一个子节点，如果当前节点没有子节点，则返回null（注意，不是undefined）。

```
<p id="para-01">
    <span>First span</span>
</p>
<p id="para-02"><span>First span</span> </p>

<script type="text/javascript">
    console.log(
            document.getElementById('para-01').firstChild.nodeName,
            document.getElementById('para-02').firstChild.nodeName
    ) ;  //??
</script>

```
> （4）Node.nextSibling

Node.nextSibling属性返回紧跟在当前节点后面的第一个同级节点。如果当前节点后面没有同级节点，则返回null。注意，该属性还包括文本节点和评论节点。因此如果当前节点后面有空格，该属性会返回一个文本节点，内容为空格。


```
<div id="div-01">Here is div-01</div>
<div id="div-02">Here is div-02</div>

<script type="text/javascript">
    var el = document.getElementById('div-01').nextSibling,
        i = 1;
    while (el) {
      console.log(i + '. ' + el.nodeName);  //??
      el = el.nextSibling;
      i++;
    }
</script>
```
下面两个表达式指向同一个节点。

```
//???
document.childNodes[0].childNodes[1]
document.firstChild.firstChild.nextSibling
```
> （5）Node.previousSibling

previousSibling属性返回当前节点前面的、距离最近的一个同级节点。如果当前节点前面没有同级节点，则返回null。

```
<div><span id="b1"></span><span id="b2"></span></div>

<script type="text/javascript">
    document.getElementById("b1").previousSibling;     // null
    document.getElementById("b2").previousSibling.id;  // "b1"
</script>
```


---




# 2、节点操作 （节点对象的一些方法）

方法是我们可以在节点（HTML 元素）上执行的动作。

列举一些常用的：

###  2.1、查找元素节点

- document.getElementById(id) ，返回带有指定 ID 的元素。
- document.getElementsByName(name)，返回带有指定名称的对象的集合。（查询元素的 name 属性）
- document.getElementsByTagName(tagname)，返回带有指定标签名的对象的集合。
- document.getElementsByClassName(classname),返回文档中所有指定类名的元素集合，作为 NodeList 对象。



2. getElementsByClassName() 在 Internet Explorer 5,6,7,8 中无效。

**栗子：**
 
```
<p>段落1</p>
<div id="main">
    <p>段落2</p>
    <p>段落3</p>
</div>
<input type="text" name="myInput">
<div class="color example">classname1</div>
<div class="color">classname2</div>


document.getElementById("main");
document.getElementsByName("myInput");
document.getElementById("main").getElementsByTagName("p");
document.getElementsByClassName("example color");

```

###  2.2、创建节点
Node.appendChild() 方法将一个节点添加到指定父节点的子节点列表末尾。

如需向 HTML DOM 添加新元素，必须首先创建该元素（元素节点），然后向一个已存在的元素追加该元素。


**栗子：**

```
<div id="div1">
    <p id="p1">段落1</p>
    <p id="p2">段落2</p>
</div>


<script>
    var para=document.createElement("p");
    var node=document.createTextNode("段落3");
    para.appendChild(node);

    var element=document.getElementById("div1");
    element.appendChild(para);
    
    //在节点前面插入
    document.body.insertBefore(node, element);
</script>
```

**解析：**
- createElement，创建新的 <p> 元素
- createTextNode，如需向 <p> 元素添加文本，必须首先创建文本节点。
- appendChild，向 <p> 元素追加这个文本节点
- 向一个已有的元素追加这个新元素。
- Node.insertBefore()， 在当前节点的某个子节点之前再插入一个子节点。
- insertBefore 方法与 appendChild 方法不同：appendChild方法传递一个参数，由依附点来调用，传递要挂载的段落；insertBefore 方法由document.body调用，传递两个参数，要挂在的段落内容 和 挂载的依附点名称，第一个参数为包含要添加内容的段落，第二个参数为依附点，即将第一个参数的段落插在第二个参数对象之前。


###  2.2.1、DocumentFragment(创建文档碎片节点)
在更新少量节点的时候可以直接向document.body节点中添加，但是当要向document中添加大量数据是，如果直接添加这些新节点，这个过程非常缓慢，因为每添加一个节点都会调用父节点的appendChild()方法，为了解决这个问题，可以创建一个文档碎片，把所有的新节点附加其上，然后把文档碎片一次性添加到document中。

**栗子：**

创建十个段落，使用常规的方式
```
//调用了十次document.body.appendChild()，每次都要产生一次页面渲染。
for(var i = 0 ; i < 10; i ++) {
    var p = document.createElement("p");
    var oTxt = document.createTextNode("段落" + i);
    p.appendChild(oTxt);
    document.body.appendChild(p);
}
```

创建文档碎片节点

```
var oFragment = document.createDocumentFragment();
for( var i = 0 ; i < 10; i ++ ) {
    var p = document.createElement("p");
    var oTxt = document.createTextNode("段落" + i);
    p.appendChild(oTxt);
    oFragment.appendChild(p);
}
document.body.appendChild(oFragment);
```



创建十个段落，使用常规的方式

###  2.3、删除节点
 删除 HTML 元素，您必须首先获得该元素的父元素：

**方法**：Node.removeChild()，从DOM中删除一个子节点。返回删除的节点。

**栗子：**

```
<div id="div1">
    <p id="p1">段落1</p>
    <p id="p2">段落2</p>
</div>


<script>
    //先定位父节点,然后删除其子节点
    //如果不是child不是parent的子节点会报错
    var parent=document.getElementById("div1");
    var child=document.getElementById("p1");
    parent.removeChild(child);
    
    
    //无须定位父节点,通过parentNode属性直接删除自身
    var child=document.getElementById("p1");
    if (child.parentNode) {
      child.parentNode.removeChild(child);
    }
    
    
    // 移除一个元素节点的所有子节点
    var element = document.getElementById("div1");
    while (element.firstChild) {
        element.removeChild(element.firstChild);
    }
</script>
```
####  2.3、替换
replacedNode = parentNode.replaceChild(newChild, oldChild); ，用指定的节点替换当前节点的一个子节点，并返回被替换掉的节点。

```
<div id="div1">
    <p id="p1">段落1</p>
    <p id="p2">段落2</p>
</div>

<script>
    // 获得被替换节点和其父节点的引用.
    var p1 = document.createTextNode("我就是要换掉段落2");
    var p2 = document.getElementById("p2");
    var parentDiv = p2.parentNode;
    
    // 用新的p元素p1来替换掉p2
    parentDiv.replaceChild(p1, p2);
</script>

```
###  2.4、修改

#### （1）创建 HTML 内容

```
<p id="p1">Hello World!</p>

<script>
document.getElementById("p1").innerHTML="New text!";
</script>
```
#### （2）改变 HTML 样式

```
document.getElementById("p1").style.color="blue";
```

### 2.5、Node.hasChildNodes()
Node.hasChildNodes方法返回一个布尔值，表示当前节点是否有子节点。

```
//如果foo节点有子节点，就移除第一个子节点。
var foo = document.getElementById("foo");

if (foo.hasChildNodes()) {
  foo.removeChild(foo.childNodes[0]);
}
```

### 2.6、Node.cloneNode()
Node.cloneNode方法用于克隆一个节点。它接受一个布尔值作为参数，表示是否同时克隆子节点，默认是false，即不克隆子节点。


```
var cloneUL = document.querySelector('ul').cloneNode(true);
```




### The slow crawling road of whiteMouse...