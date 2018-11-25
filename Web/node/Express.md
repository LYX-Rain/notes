# NodeJs Express框架

## 安装&基本用法

```bash
npm init -y
npm install express --save
```

```js
// 1.创建服务
const express = require('express');
ver server = express();
// 3.处理请求
server.use('/', function (req,res) {
  res.send('home');
  res.end();
});
// 2.监听
server.listen(8080);
```

## req & res

- Express 采用非侵入式，原来有的功能都保留，并且添加了一些功能

### req

- 接收请求的3种方式

#### get

```js
server.get('/', function(req, res){
  var name = req.query['name'];
  var password = req.query['password'];
  // 可以直接使用解构赋值
  var {name, password} = req.query;
})
```

### res

* 原生：
- res.write()
- res.end()
* express：
- res.write()
- res.end()
- res.send()    // write() + end() 传参范围更广

## cookie & session

### cookie

- cookie 空间非常小(4k)、安全性非常差

#### 发送 cookie

```js
res.cookie(name, value, {可选参数});
// 可选参数：
path: '/'           // cookie对应的路径，向上继承：根目录可以读取底层目录，反之不行
maxAge: 1000*60*60  // 有效时间 单位是毫秒
signed: false(true) // 是否需要签名（用于检验 cookie 是否被人为修改）会使数据占更多空间，默认是 false，如果需要使用要设置“密钥”(req.secret)

req.secret = 'dsaffaedvfgr';
res.cookie('user', 'Rain', {path: '/',maxAge: 1000*60*60*24,signed: true}); //在根目录添加 cookie 过期时间为1天，使用定义的字符串为签名
```

#### 读取 cookie

- 需要使用中间件（cookie-parser）解析

> npm install cookie-parser

```js
const cookieParser = require('cookie-parser');
server.use(cookieParser());
server.use(cookieParser('密钥'))  //如果另外设置了签名，在解析时需要传入签名时使用的密钥，没有另外设置的话 cookieParser 会自动设置
server.use('/', function(req, res){
  req.secret = "密钥"
  var cookies = req.cookies               // 未使用签名的 cookie
  var cookiesSecret = req.signedCookies;  // 使用过签名的 cookie
})
```

#### 删除 cookie

```js
res.clearCookie('name');
```

#### cookie 加密 [cookie-encrypter](https://www.npmjs.com/package/cookie-encrypter)（一般不使用）

> npm install cookie-encrypter

```js
const cookieParser = require('cookie-parser');
const cookieEncrypter = require('./cook');
app.use(cookieParser(secretKey));
app.use(cookieEncrypter(secretKey));
// Set encrypted cookies
res.cookie('supercookie', 'my text is encrypted', cookieParams);
res.cookie('supercookie2', { myData: 'is encrypted' }, cookieParams);
```

### session

- session 与 cookie 功能类似，但是，是储存在服务器上的，相对于 cookie 更安全
- session 是基于 cookie实现的：
1. 客户端第一次访问服务器时，服务器向客户端返回的 cookie 中有一个 sessionId
1. 客户端下一次访问时，会传给服务器那个 sessionId，服务器根据 sessionId 读取对应的 session 数据

#### 使用

- 需要通过 cookie-session 中间件使用
> npm install cookie-session

```js
server.use(cookieParser());
server.use(cookieSession({
  key: [.., .., ...]          // 必选 keys 签名 session 会从数组中循环调用，数组越长安全性越高 可以使用随机数
  name: ""                    // 可选 储存在 cookie 中的 sessionId
  maxAge: 24*3600*1000        // 可选 过期时间 默认为20分钟
}));
server.use('/', function(req, res){
  req.session
})
//删除
delete req.session;
```

## 读取静态资源

```js
server.use(static('./www'))   // 传入静态资源所在的目录
```

## 数据解析（[body-parser](https://www.npmjs.com/package/body-parser)）

- 解析 post数据 application/x-www-form-urlencoded
> npm install body-parser
- 解析完的数据会放在 req.body 内

```js
const bodyParser = require('body-parser');
server.use(bodyParser.urlencoded());
server.use('/', function(req, res){
  var data = req.body;
})
```

## 文件上传接收（[multer](https://github.com/expressjs/multer/blob/master/doc/README-zh-cn.md)）

- 需要使用中间件 multer 来解析 post 文件 multipart/form-data
> npm install multer --save
- Multer 接受一个 options 对象，其中最基本的是 dest 属性，这将告诉 Multer 将上传文件保存在哪。如果你省略 options 对象，这些文件将保存在内存中，永远不会写入磁盘。
- 以下是可以传递给 Multer 的选项

| Key | Description |
| --- | ----------- |
| dest or storage | 在哪里存储文件 |
| fileFilter | 文件过滤器，控制哪些文件可以被接受 |
| limits | 限制上传的数据 |
| preservePath | 保存包含文件名的完整文件路径 |

- 通常，只需要设置 dest 属性

```js
const multer = require('multer');
const objMulter = multer({ dest: 'uploads/' })   //指定文件上传目录
app.post('/photos/upload', objMulter.array('photos', 12), function (req, res, next) {
  // req.files 是 `photos` 文件数组的信息
  // req.body 将具有文本域数据，如果存在的话
})
```

- Multer 会添加一个 body 对象 以及 file 或 files 对象 到 express 的 request 对象中。 body 对象包含表单的文本域信息，file 或 files 对象包含对象表单上传的文件信息。
- 每个文件具有下面的信息:

| Key | Description | Note |
| --- | ----------- | ---- |
| fieldname | Field name 由表单指定 | |
| originalname | 用户计算机上的文件的名称 | |
| encoding | 文件编码 ||
| mimetype | 文件的 MIME 类型 | |
| size | 文件大小（字节单位）||
| destination | 保存路径 | DiskStorage |
| filename | 保存在 destination 中的文件名 | DiskStorage |
| path | 已上传文件的完整路径 | DiskStorage |
| buffer | 一个存放了整个文件的 Buffer | MemoryStorage |

```js
server.use(objMulter.any());            // 接受任何上传
server.use(objMulter.single('file'));   // 直接收 <input type='file' name='file'> 的上传
```

- 为了避免命名冲突，Multer 会修改上传的文件名
- 给添加扩展名（需要使用 path 和 fs 模块）

```js
server.post('/', function(req, res){
  var newName = req.files[0].path + path.parse(req.file[0].originalname).ext;
})
```

## Router 路由

## 连接数据库（MySQL）

1. 下载 mysql 模块（client）
  > npm install mysql --save
2. 建立连接
  ```js
  const mysql = require('mysql');
  var connection = mysql.createConnection({
    host     : 'localhost',
    user     : 'me',
    password : 'secret',
    database : 'my_db'
  });
  
  connection.connect();
  
  connection.query('SELECT * FROM user_table', function (error, data) {
    if (error) throw error;
    console.log(data);
  });
  
  connection.end();
  ```