# ES6标准入门读书笔记

## let 和 const 命令

### let

#### 基本用法

- let声明的变量只在let命令所在的代码块内有效

```javascript
{
    let a = 10;
    var b = 1;
}
a   //ReferenceError: a is not defined
a   //1
```

- for循环计数器很适合使用let命令

```javascript
for (let i=0;i<10;i++){
    ....
}
```

- 当前的i只在本轮循环有效。所以每一次循环的i其实都是一个新的变量。但JavaScript引擎内部会记住上一轮循环的值并以此初始化本轮变量i。
- 另外，for循环有个特别之处，设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

#### 不存在变量提升

- var 命令会发生“变量提升”现象，即可以在声明之前使用，值为 undefined。而let不行

```javascript
//var情况
console.log(foo);   //脚本开始运行时foo便已经存在，但是没有值，所以输出undefined
var foo = 2;

//let情况
console.log(bar);   //报错ReferenceError
let bar = 2;
```

#### 暂时性死区

- 只要块级作用域内存在 let 命令，它所声明的变量就“绑定”(binding) 这个区域，不会再受外部的影响

```javascript
var tmp = 123;
if(true){
    tmp = "abc";    //ReferenceError
    let tmp;        //let 将 tmp 绑定这个块级作用域，所以在 let 声明变量前，对 tmp 赋值会报错
}
```

- 在 let 声明变量前，对该变量来说都是暂时性死区

```javascript
function bar (x=y,y=2){
    return [x,y];
}
bar();  //报错
```

#### 不允许重复声明变量

```javascript
function func(arg){
    let arg;    //报错
}
function func(arg){
    {
        let arg;    //不报错
    }
}
```

### 块级作用域

#### 为什么需要块级作用域

- ES5 只有全局作用域和函数作用域，没有块级作用域这可能导致：
1. 内层变量可能会覆盖外层变量

    ```javascript
    var tmp = new Date();
    function f(){
        console.log(tmp);
        if(false){
            var tmp = "hello world";
        }
    }
    f();    //undefined
    ```

1. 用来计数的循环变量泄露为全局变量
    ```javascript
    var s = "hello";
    for(var i=0;i<s.length;i++){
        console.log(s[i]);
    }
    console.log(i); //5 ,循环结束后，i并没有消失而是泄露成了全局变量
    ```

#### ES6的块级作用域

- let 实际上为 JavaScript 新增了块级作用域

```javascript
function f1(){
    let n=5;
    if(true){
        let n=10;
    }
    console.log(n); //5
}
```

- ES6 允许块级作用域任意嵌套