---
title: 使用webpack
date: 2022-03-24 18:13:51
categories:
 - [计算机科学,前端技术,Vue]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
 - webpack
---

# webpack

## 前端工程化与webpack

前端自动化(半自动化)工程主要解决以下问题:
- JavaScript、CSS代码的合并和压缩
- CSS预处理: Less、Sass、Stylus的编译。
- 生产雪碧图(CSS Sprite)。
- ES 6转ES 5
- 模块化
····

webpack模块化示意图:
![webpack1](/assets/vue/webpack1.pnt)

左边是业务中写的各种格式的文件，比如typescript、less、jpg。这些格式的文件通过特定的加载器(Loader)编译后，最终统一生成js、css、png等静态资源文件。weppack就是用来处理这些模块(Module)之间彼此存在的依赖关系并进行打包的。
webpack的主要使用场景是单页面富应用(SPA)。SPA通常是有一个html文件和一堆按需加载的js组成。

export和import用来导出和导入模块。一个模块就是一个js文件，它拥有独立的作用域，里面定义的变量外部是无法获取的。
```javascript
//config.js
var Config = {
    version: '1.0.0'
};
export {Config};
//或:
//config.js
export var Config = {
    version: '1.0.0'
}
//其他类型(比如函数、数组、常量等)也可以导出，比如一个函数:
//add.js
export function add(a, b){
    return a + b;
};
```
模块导出后，在需要使用的文件使用import再导入
```javascript
//main.js
import {Config} from './config.js';
import {add} from './add.js';

console.log(Config); //{ version: '1.0.0'}
console.log(add(1, 2)); //2
```
可以使用export default来输出默认的模块，把模块的功能拿来使用，或者想自定义名称。
```javascript
//config.js
export default {
    version: '1.0.0'
};

//add.js
export default function(a, b){
    return a + b;
}

//main.js
import conf from './config.js;
import Add from '/add.js';

console.log(conf); //{version: '1.0.0'}
console.log(Add(1, 1)); //2
```

如果使用npm安装了一些库，在webpack中可以直接导入:
```javascript
import Vue from 'vue';
import $ from 'jquery';
```
上例分别导入了Vue和jQuery的库，并且命名为Vue和$。

## webpack基础配置

### 安装webpack与webpack-dev-server

1、创建一个目录，使用NPM初始化配置:
`npm init`
2、本地局部安装webpack:
`npm install webpack --save--dev`
--save-dev会作为开发依赖来安装webpack。安装成功后，在package.json中会多一项配置:
```json
"devDependencies" {
    "webpack": "^2.3.2"
}
```
3、安装webpack-dev-server，他可以在开发环境中提供很多服务，比如启动一个服务器、热更新、接口代理等，配置起来也很简单。在本地局部安装
`npm install webpack-dev-server --save-dev`

### webpack核心概念

webpack就是一个.js配置文件，你的架构好或差都体现在这个配置里，随着需求的不断出现，工程配置也逐渐完善

1、在目录DEMO下创建一个js文件: webpack.js，并初始化内容:
```javascript
var config = {

};

module.exports = config;
//相当于export default config, 未安装ES6的编译插件，不能使用ES6语法
```
2、在package.json的scripts里增加一个快速启动webpack-dev-server服务的脚本:
```json
{
    //...
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "webpack-dev-server --open --config webpack.config.js"
    },
    // ...
}
```
:::info no-icon
当运行npm run dev命令时，就会执行webpack-dev-server --open --config webpack.config webpack.config.js命令。其中--config是指向webpack--dev-server读取的配置文件路径，这里直接读取我们在上一步创建的webpack.config.js文件。--open会在执行命令时自动在浏览器打开页面，默认地址是127.0.0.1:8080，不过IP和端口都是可以配置的, 比如:
{
    "script": {
        "dev": "webpack-dev-server --host 172.172.172.1 --port 8888 --port 8888 --open --config webpack.config.js"
    }
}
这样访问地址就改成了172.172.172.1：8888。一般在局域网下，需要让其他同事访问时可以这样配置，否则用默认的127.0.0.1(localhost)就可以了。
:::

3、webpcak配置中最重要的也是必选的两项是入口(Entry)和出口(Output)。入口的作用是告诉webpack从哪里开始寻找依赖，并且编译，出口则是来配置编译后的文件存储位置和文件名。
在demo目录下新建一个空的main.js作为入口的文件，然后webpack.config.js中进行入口和输出的配置:
```javascript
var path = require('path');

var config = {
    entry: {
        main: './main'
    },
    output: {
        path: path.join(__dirname, './dist'),
        publishPath: '/dist/',
        filename: 'main.js'
    }
};

module.exports = config;
```
:::info no-icon
entry中的main就是我们配置的单入口，webpack会从main.js开始工作。output中path选项用来存放打包后文件的输出目录，是必填项。publicPath指定资源文件引用的目录，如果你的资源存放在CDN上，这里可以填CDN的网址。filename用于指定输出文件的名称。因此，这里配置的output意为打包后的文件会存储为demo/dist/main.js文件，只要在html中引入它就可以了。
:::

4、在demo目录下，新建一个index.html作为我们SPA的入口

```html 
<!DOCTYPE html>
<html>
    <head>
        <meta charset = "utf-8">
        <title>webpack App</title>
    </head>
    <body>
        <div id = "app">
            Hello World.
        </div>
        <script type = "text/javascript" src = "/dist/main.js"></script>
    </body>
</html>
```
5、在终端执行命令
`npm run dev`
会自动在浏览器打开页面。

6、执行下面的命令进行打包:
`webpack --progress --hide-modules`
生成一个/demo/dist/main.js文件.

### 逐步完善配置文件

对文件webpack.config.js进一步配置，实现更强大的功能。
在webpack里，每个文件都是一个模块，如.css、.js、.html、.jpg、.less等。对于不同的模块，需要用不同的加载器(Loaders)来处理，而加载器就是webpack最重要的功能。通过安装不同的加载器可以对各种后缀名的文件进行处理，比如现在要写一些CSS样式，就要用到style-loader和css-loader。
通过NPM安装:
`npm install css-loader --save-dev`
`npm install style-loader --save-dev`

安装完成后，在webpack.config.js里配置Loader，增加对.css文件的处理:
```javascript
var config = {
    // ...
    module: {
        rules: [
            {
                test: /\.css/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
};
module.exports = config;
```
:::info no-icon
在module对象的rules属性中可以指定一系列的loaders, 每一个loader都必须包含test和use两个选项。这段配置的意思是说，当webpack编译过程中遇到require()或import语句导入一个后缀名为.css的文件时，先将它通过css-loader转换，在通过style-loader转换，然后继续打包。use选项的值可以是数组或字符串，如果是数组，它的编译顺序是从后往前。
:::
CSS通过JavaScript动态创建`<style>`标签写入，意味着样式代码都已经编译在了main.js文件里，但在实际业务中，可能并不希望这样做，因为项目大了样式会很多，都放在JS里太占体积，还不能做缓存。这就要用到webpack的重要概念-插件(Plugins)。
webpack的插件功能很强大而且可以定制。可以使用一个extract-text-webpack-plugin的插件来把散落在各地的css提取出来，并生成一个main.css的文件，最终在index.html里通过<link>的形式加载它。
通过NPM安装`extract-text-webpack-plugin`插件:
`npm install extract-text-webpack-plugin --save-dev`
在配置文件中导入插件，并改写loader的配置:
```javascript
//导入插件
var ExtractTextPlugin = require('extract-text-webpack-plugin');

var config = {
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ExtractTextPlugin.extract({
                    usr: 'css-loader',
                    fallback: 'style-loader'
                })
            }
         ]
    },
    plugins: [
        //重命名提取后的css文件
        new ExtractTextPlugin("main.css")
    ]
};

module.exports = config;
```
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset = "utf-8">
        <title>webpack App</title>
        <link rel = "stylesheet" type = "text/css" href = "/dist/main.css">
    </head>
<html>
```
:::default no-icon

## 单文件组件与vue-loader