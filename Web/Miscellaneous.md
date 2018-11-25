# 遇到的问题随笔

## Js方法的笔记

### setInterval

- 丢帧 浏览器的刷新频率 60FPS
- setInterval(move,1000/60) 这是极限频率

- 队列混乱 无法保证每次函数执行间隔一致 由于底层栈和堆 时间线根据内部函数运行时间来 如果时间间隔小于函数运算时间 就无法精确定时

- this 永远指向window

### setTimeout

- 使用函数递归模拟实现绝对精确

```javascript
function add(){
    num++；
    console.log(num0);
    setTimeout(arguments.callee,1000);	//arguments.callee调用本身函数
}
```

- 获取元素本身top  element.offsetTop

- 查找e.target对应的下标 indexOf lastIndexOf

- call 函数主动执行的时候改变内部this指向

```js
function test(){
    this.doSomething
}
test.call(a)    //将test()里的this指向a
```

### 表达式：

> a&&b
- 如果a为真(true)则返回b，否则返回a

## 提高性能技巧

- 判断尽量用 ‘===’ 因为‘==’会在后台进行类型转换
- 用位运算代替parsInt()方法：  ~~(float)——>int
- for 循环预存循环的范围length，避免每次循环都去计算length
- 动态生成多个节点时，为了方便后续获取节点，先在全局创建一个空数组，在创建节点时将每个节点push进数组，获取时就可以直接使用数组而不是getElementByClassName

```javascript
var items = [];
    function creat(){
        var item = document.creatElement("div");
        items.push(item);
}
function doSomething(){
    for(var i=0,len=items.length;i<len;i++){    //预存length
        doSomething
    }
}
```

- 如果有大量的相同节点要添加相同的事件 使用事件委托

```javascript
document.body.addEventListener('click',function(e){
    if(e.target.className.toLowerCase() === 'div1')
    doSomething
})
```

## 类型检测

1. typeof:
    - 适合基本类型及function检测，遇到null失效
1. [[Class]]
    - 通过{}.toString拿到，适合内置对象和基元类型，遇到null和undefined失效（IE678等返回[obiect Object]）。
1. instanceof：
    - 检测对象 instanceof Object类型，返回true或者false
    - 适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效。
    - 判断左操作对象的原型链上是否有右边这个构造函数的prototype属性

## 缓存

- Web Storage API 提供了存储机制，通过该机制，浏览器可以安全地存储键值对，比使用 cookie 更加直观

### sessionStorage

- sessionStorage 为每一个给定的源（given origin）维持一个独立的存储区域，该存储区域在页面会话期间可用（即只要浏览器处于打开状态，包括页面重新加载和恢复）
- 在新标签或窗口打开一个页面会初始化一个新的会话，这点和 session cookies 的运行方式不同

#### 语法：

```js
// 保存数据到sessionStorage
sessionStorage.setItem('key', 'value');
// 从sessionStorage获取数据
var data = sessionStorage.getItem('key');
// 从sessionStorage删除保存的数据
sessionStorage.removeItem('key');
// 从sessionStorage删除所有保存的数据
sessionStorage.clear();
```

#### 示例

- 自动保存一个文本输入框的内容，如果浏览器因偶然因素被刷新了，文本输入框里面的内容会被恢复，因此写入的内容不会丢失

```js
// 获取文本输入框
var field = document.getElementById("field");
 
// 检测是否存在 autosave 键值
// (这个会在页面偶然被刷新的情况下存在)
if (sessionStorage.getItem("autosave")) {
  // 恢复文本输入框的内容
  field.value = sessionStorage.getItem("autosave");
}
 
// 监听文本输入框的 change 事件
field.addEventListener("change", function() {
  // 保存结果到 sessionStorage 对象中
  sessionStorage.setItem("autosave", field.value);
});
```

### localStorage

- localStorage 类似于sessionStorage。区别在于，数据存储在 localStorage 是无期限的，在浏览器关闭，然后重新打开后数据仍然存在

```js
// 储存一个 localStorage 项
localStorage.setItem(`myCat`, `Tom`);
// 读取 localStorage 项
let cat = localStorage.getItem(`myCat`);
// 移除 localStorage 项
localStorage.removeItem(`myCat`);
// 移除所有的 localStorage 项
localStorage.clear();
```