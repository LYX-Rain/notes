# JavaScript

## Ajax

### 创建 XMLHttpRequest 对象

- XMLHttpRequest 是 AJAX 的基础
- 所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。
- XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

```javascript
var xmlhttp;
if (window.XMLHttpRequest){
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp=new XMLHttpRequest();
} else {
    // IE6, IE5 浏览器执行代码
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
```

### 向服务器发送请求请求

- 如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

```javascript
xmlhttp.open("GET","ajax_info.txt",true);
xmlhttp.send();
```

| 方法 | 描述 |
| ---- | ---- |
| open(method,url,async)| 规定请求的类型、URL 以及是否异步处理请求。 |
|| method：请求的类型；GET 或 POST |
|| url：文件在服务器上的位置|
|| async：true(异步) 或 false(同步) |
|||
| send(string) | 将请求发送到服务器。 |
|| string：仅用于 POST 请求 |

- 与 POST 相比，GET更简单也更快，并且在大部分情况下都能用。
- 然而在下列情况中，使用 POST 请求：
    1. 无法使用缓存文件（更新服务器上的文件或数据库）
    1. 向服务器发送大量数据（POST 没有数据量限制）
    1. 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

#### GET 请求

```javascript
//一个简单的 GET 请求：
xmlhttp.open("GET","/try/ajax/demo_get.php",true);
xmlhttp.send();
```

- 在上面的例子中，可能得到的是缓存的结果。为了避免这种情况，向 URL 添加一个唯一的 ID：

```javascript
xmlhttp.open("GET","/try/ajax/demo_get.php?t=" + Math.random(),true);
xmlhttp.send();
```

- 通过 GET 方法发送信息，向 URL 添加信息：

```javascript
xmlhttp.open("GET","/try/ajax/demo_get2.php?fname=Henry&lname=Ford",true);
xmlhttp.send();
```

#### POST 请求

```javascript
xmlhttp.open("POST","/try/ajax/demo_post.php",true);
xmlhttp.send();
```

- 如果需要像 HTML 表单那样 POST 数据，要使用 setRequestHeader() 来添加 HTTP 头。然后在 send() 方法中规定要发送的数据：

```javascript
xmlhttp.open("POST","/try/ajax/demo_post2.php",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xmlhttp.send("fname=Henry&lname=Ford");
```

| 方法 | 描述 |
| ---- | ---- |
| setRequestHeader(header,value) | 向请求添加 HTTP 头 |
|| header：规定头的名称 |
|| value：规定头的值 |

### url - 服务器上的文件

- open() 方法的 url 参数是服务器上文件的地址：

```javascript
xmlhttp.open("GET","ajax_test.html",true);
```

- 该文件可以是任何类型的文件，比如 .txt 和 .xml，或者服务器脚本文件，比如 .asp 和 .php （在传回响应之前，能够在服务器上执行任务）。

### 异步 - True 或 False

- AJAX 指的是异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。
- XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true：

```javascript
xmlhttp.open("GET","ajax_test.html",true);
```

#### Async=true

- 当使用 async=true 时，需规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数：

```javascript
xmlhttp.onreadystatechange=function(){
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
};
xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
xmlhttp.send();
```

#### Async = false

- 如需使用 async=false，需将 open() 方法中的第三个参数改为 false：

```javascript
xmlhttp.open(&quot;GET&quot;,&quot;test1.txt&quot;,false);
```

- 当使用 async=false 时，请不要编写 onreadystatechange 函数 - 把代码放到 send() 语句后面即可：

```javascript
xmlhttp.open("GET","/try/ajax/ajax_info.txt",false);
xmlhttp.send();
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

### AJAX - 服务器 响应

- 如需获得来自服务器的响应，需要使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。

| 属性 | 描述 |
| ---- | ---- |
| responseText | 获得字符串形式的响应数据 |
| responseXML | 获得 XML 形式的响应数据 |

#### responseText 属性

- responseText 属性返回字符串形式的响应，因此可以这样使用：

```javascript
xmlhttp.onreadystatechange=function(){
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
xmlhttp.send();
```

#### responseXML 属性

- 如果来自服务器的响应是 XML，而且需要作为 XML 对象进行解析，因此使用 responseXML 属性：

```javascript
xmlhttp.onreadystatechange=function(){
    if (xmlhttp.readyState==4 && xmlhttp.status==200){
        xmlDoc=xmlhttp.responseXML;
        txt="";
        x=xmlDoc.getElementsByTagName("ARTIST");
        for (i=0;i<x.length;i++){
            txt=txt + x[i].childNodes[0].nodeValue + "<br>";
        }
        document.getElementById("myDiv").innerHTML=txt;
    }
}
xmlhttp.open("GET","test.xml",true);
xmlhttp.send();
```

## AJAX - onreadystatechange 事件

- 当请求被发送到服务器时，我们需要执行一些基于响应的任务。
- 每当 readyState 改变时，就会触发 onreadystatechange 事件。
- readyState 属性存有 XMLHttpRequest 的状态信息。
- 下面是 XMLHttpRequest 对象的三个重要的属性：

| 属性 | 描述 |
| ---- | ---- |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。|
|||
| readyState | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。|
|| 0: 请求未初始化 |
|| 1: 服务器连接已建立 |
|| 2: 请求已接收 |
|| 3: 请求处理中 |
|| 4: 请求已完成，且响应已就绪 |
|||
| status | 200: "OK" |
|| 404: 未找到页面 |

- 在 onreadystatechange 事件中，我们规定当服务器响应已做好被处理的准备时所执行的任务。
- 当 readyState 等于 4 且状态为 200 时，表示响应已就绪：

```javascript
xmlhttp.onreadystatechange=function(){
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
xmlhttp.send();
```

- onreadystatechange 事件被触发 5 次（0 - 4），对应着 readyState 的每个变化。

### 使用回调函数

- 如果网站上存在多个 AJAX 任务，那么应该为创建 XMLHttpRequest 对象编写一个标准的函数，并为每个 AJAX 任务调用该函数。
- 该函数调用应该包含 URL 以及发生 onreadystatechange 事件时执行的任务（每次调用可能不尽相同）：

```javascript
function myFunction(){
    loadXMLDoc("/try/ajax/ajax_info.txt",function(){
        if (xmlhttp.readyState==4 && xmlhttp.status==200){
            document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
        }
    });
}
```

## 服务器常用状态码

- 100——客户必须继续发出请求
- 101——客户要求服务器根据请求转换HTTP协议版本
- 200——交易成功
- 201——提示知道新文件的URL
- 202——接受和处理、但处理未完成
- 203——返回信息不确定或不完整
- 204——请求收到，但返回信息为空
- 205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
- 206——服务器已经完成了部分用户的GET请求
- 300——请求的资源可在多处得到
- 301——删除请求数据
- 302——在其他地址发现了请求数据
- 303——建议客户访问其他URL或访问方式
- 304——客户端已经执行了GET，但文件未变化
- 305——请求的资源必须从服务器指定的地址得到
- 306——前一版本HTTP中使用的代码，现行版本中不再使用
- 307——申明请求的资源临时性删除
- 400——错误请求，如语法错误
- 401——请求授权失败
- 402——保留有效ChargeTo头响应
- 403——请求不允许
- 404——没有发现文件、查询或URl
- 405——用户在Request-Line字段定义的方法不允许
- 406——根据用户发送的Accept拖，请求资源不可访问
- 407——类似401，用户必须首先在代理服务器上得到授权
- 408——客户端没有在用户指定的时间内完成请求
- 409——对当前资源状态，请求不能完成
- 410——服务器上不再有此资源且无进一步的参考地址
- 411——服务器拒绝用户定义的Content-Length属性请求
- 412——一个或多个请求头字段在当前请求中错误
- 413——请求的资源大于服务器允许的大小
- 414——请求的资源URL长于服务器允许的长度
- 415——请求资源不支持请求项目格式
- 416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
- 417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求
- 500——服务器产生内部错误
- 501——服务器不支持请求的函数
- 502——服务器暂时不可用，有时是为了防止发生系统过载
- 503——服务器过载或暂停维修
- 504——关口过载，服务器使用另一个关口或服务来响应用户，等待时间设定值较长
- 505——服务器不支持或拒绝支请求头中指定的HTTP版本

## 面向对象

- 面向对象是一种通用思想：不了解原理的情况下，会使用功能

### 面向对象编程(OOP)的特点

1. 封装：不考虑内部实现，只考虑功能使用
1. 继承：从已有对象上，继承出新的对象：多重继承、多态

### 对象的组成

1. 方法——函数：过程、动态的
1. 属性——变量：状态、静态的

- this 当前发生事件的对象（当前的方法属于谁）

### 构造函数

- 正常情况下，全局函数中的 this 是指向 window

```js
function CreatePerson(name,age){
    this.name = name;
    this.age = age;
    this.say = function (){
        aletr("My name is :" + this.name + "," + "I'm" + this.age + "years old");
    };
}
var person1 = new CreatePerson('rain',18);
var person2 = new CreatePerson('test',20);
```

- 当调用函数前加上“new”，浏览器会自动的创建一个空对象，同时将 this 指向它，并在函数结束时返回这个对象

### 原型（prototype）

- 上述程序中创建的两个 person 都具有相似的方法 say 但是这两个方法是相互独立的，占据着不同的内存空间，造成资源浪费

```js
alert(person1.say == person2.say);  //false
```

- 使用原型，让所有对象都使用相同的方法，节省资源

```js
CreatePerson.prototype.say = function (){
    aletr("My name is :" + this.name + "," + "I'm" + this.age + "years old");
}；
alert(person1.say == person2.say);  //true
```

- 在构造函数中添加属性，在原型中添加相同方法

### JSON方式（适用于单个对象）

- 整个程序里只有一个，写起来比较简单

```js
var json = {
    name: 'Rain',
    age: 18,
    say: function (){
        aletr("My name is :" + this.name + "," + "I'm" + this.age + "years old");
    }
}
```

### call()

- call() 第一个参数会成为所调用函数的 this ,后面的才是实际的传参

```js
function show(){
    aletr(this);
}
show.call(10);      //10
```

### 引用

```js
var arr1 = [1,2,3];
var arr2 = arr1;
arr2.push(4);
alert(arr1);    //1,2,3,4
alert(arr2);    //1,2,3,4
```

- 当 arr2 = arr1 时，实际上是将 arr2 也指向 arr1 所指向的空间
- 在 arr2 push 的时候 实际上是将 4 添加进它们所指向的公共空间，因此 arr1 的返回值也会发生改变

### 继承

```js
function A(){
    this.a = 12;
}
A.prototype.show = function (){
    alert(this.a);
}

function B(){
    //this -> new B()
    A.call(this);   //将 A 的 this 指向 B 的 this
}
B.prototype = A.prototype;  //B 引用了 A 的原型
B.prototype.fn = function (){
    alert('abc');
}

var objA = nwe A()
var objB = new B();
objB.fn();      //abc
objA.fu();      //abc
```

## cookie

- 以域名为单位
- 4k~10k
- 不超过50条

### 设置 cookie

```js
// cookie 格式：name = value
document.cookie = 'user = rain';    //添加
document.cookie = 'pwd = 12345';    //再次添加不会覆盖之前的添加
```

### 过期时间（expires）

- 通过 expires 设置过期时间

```js
var oDate = new Date();
oDate.setDate(oDate.getDate() + 14);
document.cookie = "user = rain;expires=" + oDate;
```

### 封装 cookie

```js
function setCookie(name, value, day){       // 传入要设置的值和过期天数
    var oDate = new Date();
    oDate.setDate(oDate.getDate() + day);   // 计算对应天数
    document.cookie = name + '=' + value + ';expires=' + oDate;
}

setCookie('user', 'rain', 14);
```

### 读取 cookie

```js
function getCookie (name){                  // 传入要读取的值
    var arr = document.cookie.split('; ');  // 拆分cookie
    for(let i=0;i<arr.length;i++){          // 循环遍历
        var arr2 = arr[i].split('=');
        if(arr2[0] == name){                // 查找要读取的名字
            return arr2[1];                 // 返回对应的值
        }
    }
    return '';                              // 如果没找到就返回空字符串
}
```

### 删除 cookie

```js
function removeCookie(name){
    setCookie(name, 1, -1)      // 设置对应的 cookie 的过期时间为 -1 天即可
}
```

## 正则表达式

### 基本方法

| 方法 | 功能 |
| ---- | ---- |
| search('') | 查找指定字符，并返回它在整个字符串中的位置；如果没找到就返回 -1 |
| substring(m,n) | 用于截取字符串，返回以 m: 起点，n: 终点的字符串（不包括终点字符） |
| charAt(n) | 返回第 n 位的字符 |
| split('') | 分割字符串，返回数组 |