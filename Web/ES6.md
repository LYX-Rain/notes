# ES6 笔记

## let 和 const

### let

- let用来声明变量，但所声明的变量，只在let命令所在的代码块内有效

```javascript
{
    var a = 100;
    let b = 200;
}
console.log(a);     // 100
console.log(b);     // b is not defined
```

- let不像var，不存在变量提升
- var 命令会发生“变量提升”现象，即可以在声明之前使用，值为 undefined。而let不行
- 在 let 声明变量前，对该变量来说都是暂时性死区
- 只要块级作用域内存在let命令，它所声明的变量就“绑定”(binding)这个区域，不再受外部影响
- let不允许在相同作用域内，重复声明同一个变量

### 块级作用域

- let 实际上为 JavaScript 新增了块级作用域
- 本质上，块级作用域是一个语句，将多个操作封装在一起，没有返回值
- 应避免在块级作用域内声明函数，如果需要应写成函数表达式的形式

```js
{   // 函数表达式
    let a = 'secret';
    let f = function () {
        return a;
    }
};
{   // 函数声明语句
    let a = 'secret';
    function f(){
        return a;
    }
}
```

### const

- const 声明一个只读常量，一旦声明，常量的值就不能改变
- 因此 const 一旦声明，就必须立即初始化赋值，只声明不赋值就会报错
- const 的作用域与 let 相同
* 本质上 const 实际保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动
* 对于简单类型的数据（数值、字符串、布尔值）而言，值就保存在变量指向的内存地址中因此等同于常量
* 但对于复合类型的数据（对象、数组）而言，变量指向的内存地址保存的只是一个指针，const 只能保证这个指针是固定的，并不能控制它指向的数据结构是不是可变的

```js
const foo = {};
foo.prop = 123;     // 可以为foo添加一个属性
foo.prop    // 123
foo = {}            // 将foo指向另一个对象就会报错
```

- 常量 foo 储存的是一个地址，这个地址指向一个对象。不可改变的是这个地址，但对象本身是可变的，依然可以为其添加属性
- 数组也是同样的，数组本身可写

```js
const a = [];
a.push('Hello');    // 可执行
a.length            // 可执行
a = ['Dave']        // 报错
```

- 如果想将对象冻结，应该使用 Object.freeze 方法

```js
const foo = Object.freeze({});
```

- 除了将对象本身冻结，对象的属性也应该冻结

```js
var constantize = (obj) => {
    Object.freeze(obj);
    Object.keys(obj).forEach( (key,i) => {
        if(typeof obj[key] === 'object'){
            constantize(obj[key]);
        }
    });
};
```

### 顶层对象的属性

- 顶层对象在浏览器环境中指的是 window 对象，在 Node 环境中指的是 global 对象
- ES5 中顶层对象的属性就是全局变量

```js
window.a = 1;
var a = 1;
```

- 但在 ES6 中，let 命令、const 命令、class 命令声明的全局变量不属于顶层对象的属性

```js
let b = 1;
window.b    // undefined
```

## 变量的解构赋值

- 解构：ES6 允许按照一定模式从数组和对象中提取值，然后对变量进行赋值

### 数组的解构赋值

```javascript
var a = 1, b = 2, c = 3;
// 等同于
var [a,b,c] = [1,2,3];
// 从数组中提取值，按照对应位置对变量赋值
```

- 本质上，这种写法属于“模式匹配”，只要两边的模式相同，左边的变量就会被赋予对应的值

```js
let [foo,[[bar],baz]] = [1,[[2],3]];    // foo = 1,bar = 2,baz = 3
let [ , ,third] = [1,2,3];  // third = 3
let [x, ,y] = [1,2,3];      // x = 1,y = 3
let [a, ...c] = [1,2,3]     // a = 1,c = [2,3]
// 如果解构不成功，变量的值就等于 undefined
let [a,b,c] = [1,2]         // a = 1,b = 2,c = undefined
let [x,y,...z] = ['a']      // x = 'a',y = undefined,c = []
```

- 不完全解构：左边的匹配模式只匹配一部分右边的数组；这种情况下，解构依然可以成功

```js
let [x,y] = [1,2,3]         // x = 1,y = 2
let [a,[b],c] = [1,[2,3],4] // a = 1,b = 2,c = 4
```

- 如果等号右边不是数组（或者严格来说不是可遍历的结构），那么就会报错

```js
// 报错
let [a] = 1;
let [a] = false;
let [a] = NaN;
let [a] = undefined;
let [a] = null;
let [a] = {};
```

#### 默认值

- 解构赋值允许指定默认值

```js
var arr = [1,2,3];
var [a,b,c,d='default'] = arr;
// 等同于：
var a = arr[0];
var b = arr[1];
var c = arr[2];
if(arr[3] === undefined){var d = 'default'}
else {var d = arr[3]}
```

- 注：ES6 内部使用严格相等运算符（===）判断一个位置是否有值
- 所以，如果一个数组成员不严格等于 undefined，默认值是不会生效的

```js
let [x = 1] = [undefined];  // x = 1
let [x = 1] = [null];       // x = null
```

- 如果默认值是一个表达式，那么这个表达式是惰性求值的，只有在用到的时候才会求值

```js
function f(){
    console.log('aaa');
}
let [x = f()] = [1];    // 因为 x 能取到值，所以函数 f 根本不会执行
```

- 默认值可以引用解构赋值的其他变量，但该变量必须已经声明

```js
let [x = 1,y = x] = [];     // x = 1,y = 1
let [x = 1,y = x] = [2];    // x = 2,y = 2
let [x = 1,y = x] = [1,2];  // x = 1,y = 2
let [x = y,y = 1] = [];     // ReferenceError
// x 用到默认值 y 时，y 还没有声明
```

### 对象的解构赋值

- 数组的元素是按次序排列的，变量的取值是由它的位置决定的
- 而对象的属性没有次序，变量无需考虑顺序，但必须与属性同名才能取到对应的值

```javascript
var obj = {
    a:1,
    b:2
}
let {b,a,c} = obj;    // a = 1,b = 2,c = undefined
```

- 解构失败，变量值等于 undefined

#### 重命名

- 如果变量名与属性名不一致，可以这样处理

```javascript
var obj = {
    a:1,
    b:2
}
let {a:A, b} = obj;     // A = 1,b = 2
```

- 对象的解构赋值的内部机制是先找到同名属性，然后再赋值给对应的变量
- 实际上，对象的解构赋值是这种形式的简写：

```js
let {foo: foo, bar: bar} = {foo: "aaa", bar: "bbb"};
```

```js
let {foo: baz} = {foo: "aaa", bar: "bbb"};  // baz = "aaa", foo: error: foo is not defined
```

- foo 是匹配的模式，baz 才是变量；真正被赋值的是变量 baz

#### 嵌套结构也可以进行解构

```js
var obj = {
    arr: [
        'test'
        { a: 1 }
    ]
}
let {arr:[test, {a}]}= obj      // test='test' a=1
```

- 注：arr 是模式，不是变量。不会被赋值。如果要 arr 也作为变量赋值，可以：

```js
var obj = {
    arr: [
        'test'
        { a: 1 }
    ]
}
let {arr, arr:[test, {a}]} = obj      // test='test', a=1, arr=['test',{a:1}]
```

- 将一个已经声明的变量用于解构赋值，需要用 () 将表达式括起来避免 JavaScript 将第一个 {} 解释为代码块（当 {} 在行首 JavaScript 会将其解释为代码块）

```js
let obj = {};
let arr = [];

({ foo: obj.prop, bar: arr[0] } = { foo: 123,bar: true });  // obj = {prop:123} , arr = [true]
```

- 当解构模式是嵌套的对象，如果子对象所在的父属性不存在，就会报错

```js
let {foo: {bar}} = {baz: 'baz'}     // 报错
```

#### 也可设置默认值

- 默认值生效的条件是，对象的属性值严格等于 undefined

```javascript
let {a=1, b=2} = {a:10}         // a=10 b=2
// 同时重命名
let {a:A=1, b=2} = {a:10}       // A=10 b=2
```

#### 解构'方法'

- 对象的解构赋值可以将现有对象的方法赋值到某个变量

```javascript
let {floor, pow} = Math;
var a = 1.9;
floor(a)            // 去小数点 a=1
pow(2,3)            // 2的3次方 返回8
```

### 字符串的解构

- 字符串也可以解构赋值，此时字符串被转换成一个类似数组的对象

```js
const [a, b, c, d, e] = 'hello';    // a = 'h',b = 'e',c = 'l',d = 'l',e = 'o'
// 还可以对 length 属性进行解构赋值
let {length: len} = 'hello';        // len = 5
```

### 数值和布尔值的解构赋值

- 解构赋值数值或者布尔值时，会先转为对象

```js
let {toString: s} = 123;
s === Number.prototype.toString     // true

let {toString: s} = true;
s === Boolean.prototype.toString    // true
```

- 数值和布尔值的包装对象都有 toString 属性，因此变量 s 都能取到值

#### undefined 和 null 无法转为对象，所以对其解构赋值时会报错

### 函数参数的解构赋值

```js
function add ([x, y]){
    return x + y;
}
add([1,2]);     // 3
```

- 函数 add 的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量 x 和 y

```js
[[1, 2], [3, 4]].map(([a, b]) => a + b)     // [ 3, 7 ]
```

- 函数参数的解构也可以使用默认值

```js
function move({x = 0,y = 0} = {}) {
    return [x, y];
}
move({x: 3,y: 8});  // [3, 8]
move({x: 3});       // [3, 0]
move({});           // [0, 0]
move();             // [0, 0]
```

- 注意与下面写法的区别

```js
function move({x, y} = {x: 0,y: )}){
    return [x, y];
}
move({x: 3,y: 8});  // [3, 8]
move({x: 3});       // [3, undefined]
move({});           // [undefined, undefined]
move();             // [0, 0]
```

- 上面代码是为函数 move 的参数指定默认值，而不是为变量 x 和 y 指定默认值
- undefined 就会触发函数参数的默认值

### 用途

#### 交换变量的值

```js
let x = 1;
let y = 2;

[x, y] = [y, x];
```

#### 从函数返回多个值

```js
// 返回一个数组
function f(){
    return [1, 2, 3];
}
let [a, b, c] = f();

// 返回一个对象
function f(){
    return {
        foo: 1,
        bar: 2
    };
}
let {foo, bar} = f();
```

#### 提取 JSON 数据

```js
let json = {
    id: 22,
    status: "OK",
    dara: [3213,321]
};
let {id, status, data: number} = json;
```

#### 输入模块的指定方法

```js
const { SourceMapConsumer, SourceNode } = require("source-map");
```

## 函数的拓展

### 函数参数的默认值

```js
function (x = 0,y = 0) {
    this.x = x;
    this.y = y;
}
var p = new Point();        // {x: 0,y: 0}
```

- 参数默认值是惰性求值的

## 字符串的拓展

### 字符串的遍历器接口

- ES6 为字符串添加了遍历器接口，使得字符串可以被for...of循环遍历。

```js
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```

### at()

- ES5 对字符串对象提供charAt方法，返回字符串给定位置的字符。该方法不能识别码点大于0xFFFF的字符。

```js
'abc'.charAt(0) // "a"
'𠮷'.charAt(0) // "\uD842"
```

- 上面代码中的第二条语句，charAt方法期望返回的是用2个字节表示的字符，但汉字“𠮷”占用了4个字节，charAt(0)表示获取这4个字节中的前2个字节，很显然，这是无法正常显示的。
- 目前，有一个提案，提出字符串实例的at方法，可以识别 Unicode 编号大于0xFFFF的字符，返回正确的字符。

```js
'abc'.at(0) // "a"
'𠮷'.at(0) // "𠮷"
```