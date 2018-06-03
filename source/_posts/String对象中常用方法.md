---
title: String对象中常用方法
date: 2018-04-21 22:00:10
tags:
---


### 1、charAt() 和 charCodeAt()

charAt()：返回指定索引位置处的字符。如果超出有效范围的索引值返回空字符串。

```
var str = "Hello world";

console.log(str.charAt(0));  //H
console.log(str.charAt(99)); //""
```


charCodeAt()：与charAt类似，只不过返回表示给定索引的字符的Unicode的值。如果指定的 index 小于 0 或不小于字符串的长度，则 charCodeAt 返回 NaN。


```
"ABC".charCodeAt(0) // returns 65:"A"

"ABC".charCodeAt(1) // returns 66:"B"

"ABC".charCodeAt(2) // returns 67:"C"

"ABC".charCodeAt(3) // returns NaN
```


### 2、indexOf() 和 lastIndexOf()

indexOf()：从字符串对象中返回首个被发现的给定值的索引值，如果没有找到则返回-1。

lastIndexOf()：从字符串对象中返回最后一个被发现的给定值的索引值，如果没有找到则返回-1。

接收两个参数：第一个是需要搜索的子字符串，第二个是从字符串中的哪个位置开始搜索（可选）。


```
var str = "hello world";

console.log(str.indexOf("l")); //2
console.log(str.lastIndexOf("l")); //9

console.log(str.indexOf("l",4)); //9
console.log(str.lastIndexOf("l",4)); //3
```


### 3、concat()

连接两个字符串文本，并返回一个新的字符串。

```
var hello = "Hello,";
console.log(hello.concat("good luck for you.")); /* Hello,good luck for you. */
```


在实际应用中，最常用到的还是加号操作符“+”,或者es6模板字符串``

```
var a = "Hello,";
var b = "good luck for you.";
console.log(a+b); /* Hello,good luck for you. */
console.log(`${a}${b}`); /* Hello,good luck for you. */
```

### 4、slice()、substr()、substring()

#### stringObject.slice(start,end)

摘取一个字符串区域，返回一个新的字符串。接收两个参数：第一个参数指定子字符串的开始位置，第二个参数指定的是子字符串最后一个字符后面的位置（如果没有指定第二个参数，则一直到字符串结尾）。该方法对原字符串没有影响。

```
var str = "hello world";
console.log(str.slice(2)); //llo world
console.log(str.slice(2,8)); //llo wo

console.log(str.slice(-9)); //llo world
console.log(str.slice(2,-3)); //llo wo
```

当传入的参数为负数时，会将该负数加上字符串的长度转换成正数，再进行截取。如果传入的负数的绝对值大于字符串的长度，则用0替代。

#### stringObject.substr(start,length)

在字符串中抽取从 start 下标开始的指定数目的字符。
- length如果省略了，那么返回从 stringObject 的开始位置到结尾的字串。

```
var str="Hello world!"

console.log(str.substr(3))   //lo world!
console.log(str.substr(3,4)) //lo w
```

#### stringObject.substring(start,stop)

返回在字符串中指定两个下标之间的字符。只能接收非负的整数参数
- substring() 方法返回的子串包括 start 处的字符，但不包括 stop 处的字符。
- 如果参数 start 与 stop 相等，那么该方法返回的就是一个空串（即长度为 0 的字符串）。如果 start 比 stop 大，那么该方法在提取子串之前会先交换这两个参数。

```
var str="Hello world!"

console.log(str.substring(3))   //lo world!
console.log(str.substring(3,4)) //l
```

> 注意：
与 slice() 和 substr() 方法不同的是，substring() 不接受负的参数。


### 5、trim()
ES5新增方法，用来删除字符串前置及后缀的所有空格。

```
var str = "   hello world   ";
var strCopy = str.trim();
console.log(str); //"   hello world   "
console.log(strCopy); //"hello world"
```

### 6、toLowerCase() 和 toUpperCase()

- toLowerCase() ：将整个字符串转成小写字母。 
- toUpperCase()：将整个字符串转成大写字母。
- 还有两个方法：toLocaleLowerCase()和 toLocaleUpperCase()方法则是针对特定地区的实现。
- 少数语言（如土耳其语）会为 Unicode 大小写转换应用特殊的规则，这时候就必须使用针对地区的方法来保证实现正确的转换。

```
var str = "HellO WoRlD"; 
console.log(str.toLowerCase()); //hello world
console.log(str); //HellO WoRlD(原字符串不变)
console.log(str.toLocaleLowerCase()); //hello world

console.log(str.toUpperCase()); //HELLO WORLD
console.log(str.toLocaleUpperCase()); // HELLO WORLD
```

### 7、match()
match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。

```
var myString = "welcome to my blog blog blog...";
var myPattern = /.og/;
var myResult = myString.match(myPattern); 
console.log(myResult);// ["log", index: 15, input: "welcome to my blog blog blog..."] 
console.log(myResult[0]); // "log"
console.log(myResult.index);// 15 
console.log(myResult.input);// "welcome to my blog blog blog..."

var myPattern2 = /.oog/;
var myResult2 = myString.match(myPattern2);
console.log(myResult2); //null
```

### 8、search()
返回与正则表达式查找内容匹配的第一个字符串的位置。
> 与match()方法不同的是：search()方法只返回一个数值而不是一个数组。

```
var myString = "welcome to my blog blog blog...";
var myPattern = /.og/;
var myResult = myString.search(myPattern); 
console.log(myResult);// 15
```

### 9、replace()
用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

```
var str="Mr Blue has a blue house and a blue car";
var n=str.replace(/blue/g,"red"); //Mr Blue has a red house and a red car
```

忽略大小写

```
var str="Mr Blue has a blue house and a blue car";
var n=str.replace(/blue/gi, "red");
```


### 10、split()
把一个字符串分割成字符串数组。

```
var str = "cat, bat, sat, fat";
var strArr = str.split(",");
console.log(strArr); // ["cat", " bat", " sat", " fat"]
var strArr2 = str.split(",",2);
console.log(strArr2); // ["cat", " bat"]
```

### 11、localeCompare()
- 用本地特定的顺序来比较两个字符串
- 比较两个字符串，并根据比较结果返回一个值。该方法接收一个参数，即要与之比较的字符串参数。
- 如果字符串在字母表中应该排在字符串参数之前，则返回一个负数（大多数情况下是-1）
- 如果字符串等于字符串参数，则返回 0；
- 如果字符串在字母表中应该排在字符串参数之后，则返回一个正数（大多数情况下是 1）

```
var str = "sean";
console.log(str.localeCompare("lily")); // 1
console.log(str.localeCompare("sean")); // 0
console.log(str.localeCompare("Tom"));  // -1
```

### 12、fromCharCode()
接收一或多个字符编码，然后将它们转换成一个字符串。该方法是String 构造函数本身的静态方法。该方法与前面介绍的charCodeAt()方法作用正好相反。

```
console.log(String.fromCharCode(104, 101, 108, 108, 111)); //"hello"
```

