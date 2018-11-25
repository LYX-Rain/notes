# Node.js

## 简介

- 编写高性能网络服务器的JavaScript工具包
- 单线程、异步、事件驱动
- 特点：快，耗内存多

- PHP是单线程，耗内存少速度相对慢些

## 在Linux上安装node

1. 先安装一个[nvm](https://github.com/creationix/nvm)用于切换node版本:
    - 进入nvm的GitHub点击README的installation
    - 找到下列代码（为了获取最新的版本）复制下  来输入到Ubuntu的命令行中

    ```bash
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
    ```

    - 安装完要重启一下

1. nvm安装完成后输入：

    ```bash
    nvm install --lts
    nvm use <version>(你安装的版本号)
    ```

    - 安装最新稳定版,并使用它
    - 输入node，如果进入了node交互环境就安装成功了
    - 如果要查看已经安装的node版本，输入：

    ```bash
    nvm ls
    ```

1. 完善安装

    - 上述过程完成后，有时会出现，当开启一个新的 shell 窗口时，找不到 node 命令的情况，这种情况一般来自两个原因：
    - 一、shell 不知道 nvm 的存在
    - 二、nvm 已经存在，但是没有 default 的 Node.js 版本可用。

    - 解决方式：
    - 一、检查 ~/.profile 或者 ~/.bash_profile 中有没有这样两句

    ```bash
    export NVM_DIR="/Users/YOURUSERNAME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
    ```

    - 没有的话，加进去。这两句会在 bash 启动的时候被调用，然后注册 nvm 命令。
    - 二、调用

    ```bash
    nvm ls
    ```

    - 看看有没有 default 的指向。如果没有的话，执行

    ```bash
    $ nvm alias default <version>(你安装的版本号)
    #再
    $ nvm ls
    #看一下
    ```

## 使用 pm2 启动 node

```bash
npm install pm2 --global
pm2 start server.js         #最好设置监听80端口
```

- 如果在虚拟机中开启 node 在主机访问不到服务是因为虚拟机开启了防火墙, 所以物理主机没法访问到. 执行以下两个指令即可.

```bash
systemctl stop firewalld.service    #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动
```

## 基本HTTP服务器

```javascript
//server.js
var http = require('http');                     //导入Node.js自带的HTTP模块
http.createServer(function(request,response){   //调用HTTP模块的createServer()函数创建一个服务，该函数有两个参数：request和response它们是对象，用它们的方法来处理HTTP请求的细节，并且响应请求
    response.statusCode = 200;                  //返回的状态码
    response.setHeader('Content-Type', 'text/plain');   //HTTP协议头输出类型
    response.end();                             //结束HTTP请求，不写就没有HTTP协议尾
}).listen(8000);                                //监听8000端口
console.log("Server running at http://127.0.0.1:8000/")
```

- createServer()方法接受一个方法作为参数
- 如果只是上面的一个简单服务，浏览器访问时会提交两次HTTP请求(express框架中已经清除)
- 如果不用express框架，需手工清除：

```javascript
if(request.url !== "/favicon.ico"){     //清除第2次访问:因为大部分浏览器都会在你访问 http://localhost:8888/ 时尝试读取 http://localhost:8888/favicon.ico（网页标题“title”旁的图标），请求地址就是“/favicon.ico”但对于我们服务器来说这次请求是不需要进行额外处理的无效请求。
    response.write();
    response.end();
}
```

## 调用其它函数

### 调用本文件内的函数

```javascript
//server.js
var http = require('http');

http.createServer(function(request,response){
    response.statusCode = 200;
    response.setHeader('Content-Type', 'text/plain');
    test(response);     //直接调用
    response.end();
}).listen(8000);

console.log("Server running at http://127.0.0.1:8000/");

function test(res){
    res.write("hello world!")
}
```

### 调用其它文件内的函数

- 原文件

```javascript
//server.js
var http = require('http');
var test = require('./test.js')

http.createServer(function(request,response){
    response.statusCode = 200;
    response.setHeader('Content-Type', 'text/plain');
    //调用方式1：
    test.hello(response);
    test.world(response);
    //方式2：
    test['hello'](response);
    test['world'](response);
    //方法2更为常用，因为可以通过：
    var funname = 'hello';
    test[funname](response)
    //这种方式,改变字符串(funname)从而调用想要的函数
    response.end();
}).listen(8000);

console.log("Server running at http://127.0.0.1:8000/");
```

- 要调用的文件

```javascript
//test.js
//形式1：
function hello(res){
    res.write("hello");
};
function world(res){
    res.write("world");
};
module.exports = hello;     //只支持一个函数

//形式2：支持多个函数
module.exports = {
    hello: function(res){
        res.write("hello");
    },
    world: function(res){
        res.write("world");
    }
}
```

- 只有用module.exports导出，此文件内的方法才能被其他文件调用

## 模块的调用

- JavaScript中类的写法：

```javascript
//user.js
function user (id,name,age){
    this.id=id;
    this.name=name;
    this.age=age;
    this.self=function(){
        console.log(this.name+"is"+this.age+"years old");
    }
}
module.exports = user;
```

- 调用

```javascript
//server.js
var http = require('http');
var user = require('./user')

http.createServer(function(request,response){
    response.statusCode = 200;
    response.setHeader('Content-Type', 'text/plain');

    user1 = new user(1,"Rain",20);
    user1.self();

    response.end();
}).listen(8000);

console.log("Server running at http://127.0.0.1:8000/");
```

- ps: JavaScript中类的继承：

```javascript
var user = require('./user');
function admin (id,name,age){
    user.apply(this,[id,name,age]);
    this.idis=function(res){
        res.write(this.name+"is the"+this.id+"th");
    }
}
module.exports = admin;
```

## 路由

- 通过解析url获取路由的字符串，调用路由文件内对应的方法，通过该方法读取对应的HTML文件，再将HTML文件返回给客户端

```javascript
//server.js
var http = require('http');
var url = require('url');           //node.js提供一个“url”对象来解析url
var router = require('./router');   //引入路由文件

http.createServer(function(request,response){
    response.statusCode = 200;
    response.setHeader('Content-Type', 'text/plain');
    var pathname = url.parse(request.url).pathname;     //通过url.pathname方法解析出url后面的路由
    pathname = pathname.replace(/\//,'')                //通过正则将路由字符串的斜杠头给去掉
    router[pathname](request,response);                 //通过pathname字符串调用路由中对应的方法
    response.end();
}).listen(8000);

console.log("Server running at http://127.0.0.1:8000/");
```

- 路由文件：router.js

```javascript
//router.js
module.exports={
    login: function(req,res){
        res.write("转到login页面")
    }
    register: function(req,res){
        res.write("转到注册页面")
    }
}
```

## 读取文件

### 同步读取文件（不推荐）

- 读取文件的配置文件：optfile.js

```javascript
//optfile.js
var fs = require('fs');                 //node.js提供的操作文件模块
module.exports={
    readfileSync: function (path) {     //同步读取方法
        var data = fs.readFilesSync(path,'utf-8');  //通过fs的readFilesSync方法同步读取文件，path为文件路径，以utf-8的编码读取
        return data;
    },
    readfile: function (path) {         //异步读取方法
        fs.readFile(path,function(err,data){
            if (err){
                console.log(err);
            } else {
                return data
            };
            console.log("函数执行完毕");
        });
    }
}
```

```javascript
//server.js
var http = require('http');
var optfile = require('./optfile.js');  //引入配置文件

http.createServer(function(request,response){
    response.statusCode = 200;
    response.setHeader('Content-Type', 'text/plain');
    optfile.readfileSunc('.src/login.html')   //路径是相对于此文件的
    response.end("主程序执行完毕");
}).listen(8000);

console.log("Server running at http://127.0.0.1:8000/");
```

- node.js的高性能主要就是依靠异步操作，在异步时，当服务器执行读文件的操作时程序会继续执行下去，而不是在原地等文件读取完毕，因此上述代码会在控制台依次输出：

```bash
函数执行完毕
主程序执行完毕         #文件还未读取完主程序就已经结束了(response.end)
（最后返回读取到的文件内容“data”）
```

- 而正是由于这种异步操作，如果想要将读取的文件内容返回给客户端，就要使用“闭包”的方式

```javascript
//server.js
var http = require('http');
var optfile = require('./optfile.js');  //引入配置文件

http.createServer(function(request,response){
    response.statusCode = 200;
    response.setHeader('Content-Type', 'text/plain');
    function recall(data){              //创建闭包函数
        response.write(data);           //它可以储存response
        response.end();
    }
    optfile.readfileSunc('.src/login.html',recall)   //路径是相对于此文件的
}).listen(8000);

console.log("Server running at http://127.0.0.1:8000/");
```

```javascript
//optfile.js
var fs = require('fs');
module.exports={
    readfileSync: function (path) {     //同步读取方法
        var data = fs.readFilesSync(path,'utf-8');
        return data;
    },
    readfile: function (path,recall) {         //异步读取方法,接入闭包函数
        fs.readFile(path,function(err,data){
            if (err){
                console.log(err);
            } else {
                recall(data);                   //因为是闭包函数recall会储存response
            };
            console.log("函数执行完毕");
        });
    }
}
```

## 写文件

- 在optfile.js中加入写文件功能：

```javascript
//optfile.js
var fs = require('fs');
module.exports = {
    writefile: function (path,recall) {
        fs.readFile(path, function(err,data){       //异步方式
            if(err){
                console.log(err)
            } else {
                console.log("It's saved!")；         //文件被保存
                recall("写入文件成功")；
            }
        })
    }
}
```

- 在server.js中加入：

```javascript
optfile.writefile("./src/data.json","这里传入想要写入的数据",recall);
```

- PS：实际开发过程中,这些函数一般是不放在server.js中的，应该放在路由配置文件中，配置相应的写数据接口

## 读取并显示图片

- 在optfile.js中加入写文件功能：

```javascript
readImg: function(path,res){
    fs.readFile(path,"binary",function(err,filedata){   //binary参数代表读取的是一个二进制流的文件
        if(err){
            console.log(err)；
            return;
        } else {
            res.write(filedata,"binary");               //用“binary”的格式发送数据
            res.end();
        }
    })
}
```

- 这种方法只能单独读取图片

## 参数接受

- 只需修改路由文件中的：

```javascript
var querystring = require('querystring');       //需要引入querystring模块来解析POST请求体中的参数

confirm: function (req,res) {
    //get方式接收参数，处理是同步的
    var rdata = url.parse(req.url,true).query;
    if(rdata['email'] != undefined){
        console.log("email:"+rdata['email']+","+"password:"+rdata['password']);
    };

    //post方式接收参数，处理是异步的
    var post = '';
    req.on('data',function(chunk){
        post += chunk;
    });
    req.on('end',function(){
        post = querystring.parse(post);
        console.log(post['email']+","+post['password']);
    });
}
```

## 动态数据渲染

- 在使用路由传输文件的时候如果想要将页面中某些字段动态的替换成对应的数据，可以在数据(data)传输时先将其字符化(toString())，再使用正则将匹配到的标识字符进行替换，如vue中就使用"{{}}"双大括号标识数据位置，再进行匹配替换为对应数据
- 如修改回调函数

```javascript
function recall(data){
    dataStr = data.toString();
    for(let i=0;i<arr.length;i++){          //数组arr中储存需要替换的数据名
        re = new RegExp("{"+arr[i]+"}",g);  //用正则匹配标识数据（这里我用“{}”一个大括号标识数据）
        dataStr = dataStr.replace(re,post[arr[i]])  //从数据库中循环替换对应的数据
    }
    res.write(dataStr);
    res.end();
}
```

## 异步流程控制

- 当有某些操作需要依赖于上一步操作完成后才能执行，因为node里的操作大都是异步的，这就需要对异步流程进行控制
- node.js提供了一个异步流程控制对象async

1. 串行无关联：async.series   (依次执行，此步执行的结果不影响下一个程序)
1. 并行无关联：async.parallel (同时执行，正常的异步)
1. 串行有关联：waterfall  (瀑布流)

- async需要另外安装：

```bash
npm install async --save-dev
```

## 连接MySQL数据库

### 直接连接（不常用，需了解）

- 安装MySQL支持：

```bash
npm install mysql
```

- 创建一个新的数据库，建表：

```mysql
create table user(
    uid int not null primary key auto_increment,
    uname varchar(100) not null,
    pwd varchar(100) not null
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

- 直接连接MySQL并进行操作，下面列举了一些常用的操作，在实际操作过程中一般把这些操作封装到对应的方法中，需要的时候直接调用

```javascript
var mysql = require("mysql")    //调用MySQL模块
//创建一个connection
var connection = mysql.createConnection({
    host: 'localhost',  //主机
    user: 'root',       //MySQL认证用户名
    password: "",       //MySQL认证用户密码
    database: "rain",   //数据库名
    port: '3306'        //端口号 默认 3306 没修改可不用设置
});
//打开connection连接
connection.connect(function(err){
    if(err){
        console.log('[query] - :'+err);
        return;
    }
    console.log("[connection connect] succeed");
})

//向表中插入数据
var userAddSql = "insert into user (uname,pwd) values(?,?)";    //要执行的MySQL语句，value的问号是占位符
var param = ['test','test'];                                    //要插入的数据可以通过变量传值，这里用字符串测试
connection.query(userAddSql,param,function(err,rs){             //调用query方法插入数据，第一个参数是执行的MySQL语句，第二个是插入的数据，第三个是匿名回调函数处理报错；传的值一定要和MySQL语句匹配
    if(err){
        console.log("insert err:",err.message);
        return;
    }
    console.log(rs);    //rs是插入成功后返回的一些参数
});

//执行查询
//群体查询
connection.query('SELECT * from user',function(err,rs,fields){
    if(err){
        console.log('[query] - :'+err);
        return;
    }
    for(let i=0;i<re.length;i++){       //re为结果集
        console.log('The solution is: ',rs[i].uname);
    }
    console.log(fields);        //fields为查询产生的信息，不是查询结果，一般没什么用
})
//单独查询
connection.query('SELECT * from user where uid=?',[2],function(err,rs,fields){  //查询uid=2的那条数据
    if(err){
        console.log('[query] - :'+err);
        return;
    }
    for(let i=0;i<re.length;i++){
        console.log('The solution is: ',rs[i].uname);
    }
    console.log(fields);
})

//关闭connection连接
connection.end(function(err){
    if(err){
        console.log(err.toString());
        return;
    }
    console.log("[connection end] succeed");
})
```

- 这里列举的操作都是较为常用的，还有其它的操作可以直接通过修改传入的MySQL语句来完成

### 连接池连MySQL（常用，效率高）

- 原理：
- 创建MySQL连接的开销十分巨大
- 使用连接池 server启动时会创建10-20个连接放在连接池中，当有访问需连接MySQL数据库时，就从连接池中取出一个连接，进行数据库操作，操作完成后，再将连接放回连接池
- 连接池会自动的管理连接，当连接较少时，会减少连接池中的连接，当连接量较大时，会扩充连接
- 使用node提供的连接池需安装node-mysql模块：

```bash
npm install node-mysql -g
```

#### 操作连接池

```javascript
//optPool.js
var mysql = require('mysql');
function optPool(){         //创建一个连接池的类方便使用
    this.flag = true;       //用来标记是否连接过
    this.pool = mysql.createPool({
        host: 'localhost',  //主机
        user: 'root',       //MySQL认证用户名
        password: "",       //MySQL认证用户密码
        database: "rain",   //数据库名
        port: '3306'        //端口号
    });
    this.getPool = function(){  //初始化pool
        if(this.flag){
            //监听connection事件
            this.pool.on('connection',function(connection){
                connection.query('SET SESSION auto_increment_increment=1');
                this.flag = false;
            });
        }
        return this.pool;
    }
};
module.exports = optPool;   //导出为一个类
```

```javascript
var optPool = require('./optPool');
var optpool = new optPool();
var pool = optpool.getPool();

//从连接池中获取一个连接
pool.getConnection(function(err,connect){   //如果操作成功就拿到连接（connect）
    //做一个插入操作
    var userAddSql = "insert into user (uname,pwd) values(?,?)";
    var param = ['test','test'];
    connect.query(userAddSql,param,function(err,rs){    //异步操作
        if(err){
            console.log("insert err:",err.message);
            return;
        }
        console.log('success');
        connect.release()   //将连接放回连接池
    });
})
```