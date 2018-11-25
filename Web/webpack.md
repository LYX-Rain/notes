# Webpack笔记

- 本质上，webpack 是一个现代 JavaScript 应用程序的模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

## webpack的安装及简单使用

```bash
npm install -g webpack          #通过 npm 全局安装 webapck
npm init                        #始化 package.json 文件
npm install webpack --save-dev  #在项目中安装 webpack
```

- --save-dev 是开发时候依赖的东西，-save 是发布之后还依赖的东西
- 在项目中创建如下文件结构：

```bash
├── index.html  # 显示的网页
├── main.js     # webpack 入口
└── bundle.js   # 通过 webpack 命令生成的文件，无需创建
```

### 通过命令对项目中依赖的js文件进行打包：

- 在命令行中执行：webpack Hello.js(源文件名称) Hello.bundle.js(打包后生成文件的名称)
- 操作成功后会给出一部分信息

```bash
# webpack 要打包的 js 文件名  打包后生成的js文件名
$ webpack main.js bundle.js

#!/bin/bash
Hash: 6eccbaf93405986d574a      //哈希值
Version: webpack 3.8.1          //webpack的版本
Time: 51ms                      //打包所花费的时间
Asset(打包所生成的文件)   Size(文件大小)  Chunks(打包的分块)     Chunk Names(分块名称)
Hello.bundle.js         2.49 kB         0  [emitted]          main
   [0] ./Hello.js 21 bytes {0} [built]
```

### 在webpack命令后面还可以加入以下参数：

- --watch 实时打包
- --progress 显示打包进度
- --display-modules 显示打包的模块
- --display-reasons 显示模块包含在输出中的原因
- 更多参数可以通过命令 webpack --help 查看

## webpack 中的四个核心概念

- Entry 入口
- Output 输出
- Loaders
- Plugins 插件

- webpack 中默认的配置文件名称是 webpack.config.js，因此我们需要在项目中创建如下文件结构：

```bash
├── index.html            # 显示的页面
├── main.js               # webpack 入口
├── webpack.config.js     # webpack 中默认的配置文件
└── bundle.js             #  通过 webpack 命令生成的文件，无需创建
```

### entry 入口

- 入口起点（entry point）指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后。 webpack  会找出有哪些模块和库是入口起点（直接和间接）依赖的。
- 在 webpack.config.js 中 配置 entry  属性，来指定一个入口或多个起点入口

```javascript
moudle.exports = {
    entry: './path/file.js'
};
```

### output 输出

- output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。你可以通过在配置中指定一个 output 字段，来配置这些处理过程：

```javascript
const path = require('path');

module.exports = {
  entry: './path/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

- 其中 output.path 属性用于指定生成文件的路径，output.filename 用于指定生成文件的名称。

### Loaders

- Loaders 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后可以利用 webpack 的打包能力，对它们进行处理。
- 本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图可以直接引用模块。在更高层面上，在 webpack 的配置中 loader 有两个目标：
1. 识别应该被对应的 loader 进行转换的那些文件（使用 test 属性）
1. 转换这些文件，从而使其能够被添加到依赖图中（并且最终添加到 bundle 中）(use 属性)
- 例如css，如果想让 webpack 能够打包css文件就需要安装 style-loader 和 css-loader

```bash
npm install --save-dev style-loader css-loader
```

- 然后在 webpack.config.js 中配置使用：

```javascript
const path = require('path');

module.export = {
    entry: './main.js',
    output: {
        path: path.resolve(__dirname,'dist'),
        filename: 'bundle.js'
    },
    module: {
        rules:[
            {
                test: /\.css$/,
                use: [
                    { loader: 'style-loader' },
                    { loader: 'css-loader' }
                ]
            }
        ]
    }
};
```

- 以上配置中，对一个单独的 module 对象定义了 rules 属性，里面包含两个必须属性：test 和 use。这告诉 webpack 编译器(compiler) 如下信息：
- “嘿，webpack 编译器，当你碰到「在 require()/import 语句中被解析为 '.css' 的路径」时，在你对它打包之前，先使用 style-loader 和 css-loader 转换一下。”

### Plugin 插件

- loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。
- 想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。

```javascript
//webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

## Loader 详解

- 在配置文件(webpack.config.js)中使用,通过正则表达式匹配对应要处理的文件

```javascript
module: {
    loaders: [{
        test: /\.js$/,              //通过正则匹配需要处理文件
        loader: 'babel',            //所用的loader
        exclude: './node_modules/', //指定排除的范围加快打包速度
        include: './src/',          //指定打包的范围也可加快打包速度
        query:{                     //需要使用的其它插件
            presets: ['latest']
        }
    },
    {
        test: /\.css$/,
        loader: 'style-loader!css-loader'   //css-loader处理完后再由style-loader处理
    }
```

### 使用postcss-loader可以解决浏览器自动适配

```javascript
    {
        test: /\.css$/,
        loader: 'style-loader!css-loader(?importLoaders=n)!postcss-loader'  //如果css中有import引入,就需要括号中的传参，引入了多少个其他css,n就是多少
    }
    ]
},
postcss: [  //引入浏览器适配的模块(需要npm安装)
    require('autoprefixer')({
        broswers: ['last 5 Versions']
    })
]
```

### less和sass

```javascript
{
    test: /\.less$/,
    loader: 'style!css!postcss!less'
}
{
    test: /\.sass$/,
    loader: 'style!css!postcss!sass'
}
```

在配置文件中写绝对路径需要使用node的api

```javascript
var path = require('path');
```

所有的路径可以写成：path.resolve(__dirname, '文件名')，如

```javascript
exclude: path.resolve(__dirname, 'node_modules')
```