# Day01

## 铺垫(自学)

### 使用npm

- [ ] 熟练使用npm常用命令

> 新建文件夹练习

1. [npm](https://www.npmjs.com/)（**推荐**）

> 设置淘宝镜像

```bash
$ npm config set registry  https://registry.npm.taobao.org/  #设置淘宝镜像地址
$ npm config get registry  #查看镜像地址
```

```bash
# 1. 初始化, 得到package.json文件(当前路径下)
npm init  /  npm init -y

# 2. 添加依赖(下包)
npm i [package]
npm i [package]@[version]
# 例如: npm i jquery
# 例如: npm i jquery@3.5.1
npm i axios -S # 生产依赖 => xie
npm i less -D # 开发依赖

# 3. 移除包
npm un [package]
# 例如: npm un axios
             
# 4. 安装项目全部依赖(一般拿到别人的项目缺少node_modules)          
npm i

# 5. 全局
安装: npm i [package] -g
卸载: npm un [package] -g
# 例如 npm i @vue/cli -g
```
2. [yarn](https://www.yarnpkg.cn/)

```bash
# 1. 初始化, 得到package.json文件(当前路径下)
yarn init  /  yarn init -y

# 2. 添加依赖(下包)
yarn add [package]
yarn add [package]@[version]
# 例如: yarn add jquery
# 例如: yarn add jquery@3.5.1

# 3. 移除包
yarn remove [package]
# 例如: yarn remove jquery
             
# 4. 安装项目全部依赖(一般拿到别人的项目缺少node_modules)          
yarn 或者 yarn install

# 5. 全局
安装: yarn global add [package]
卸载: yarn global remove [package]
# 例如 yarn global add @vue/cli
```



## 今日学习目标

1. 能够理解webpack基本概念和作用
2. 能够掌握webpack使用步骤
3. 能够使用webpack相关配置打包
4. 能够使用webpack开发服务器
5. 能够查阅使用webpack中文文档
6. 服务器运行代码



## 1. webpack基本概念

> 目标: webpack本身是, node的一个第三方模块包, 用于打包代码

[webpack官网](https://webpack.docschina.org/)

* 现代 javascript 应用程序的 **静态模块打包器 (module bundler)**

* 为要学的 vue-cli 开发环境做铺垫

把很多文件打包整合到一起, 缩小项目体积, 提高加载速度(**演示准备好的例子**)

![image-20210207234927772](assets/image-20210207234927772.png)

其中功能:

* less/sass -> css

* ES6/7/8 -> ES5

* html/css/js -> 压缩合并

* Vue类型文件 -> 抽离提取html/css/js

## 2. webpack的使用步骤

### 2.0_webpack基础使用

> 目标: 把src下的2个js文件, 打包到1个js中, 并输出到默认dist目录下

默认入口: ./src/index.js

默认出口: ./dist/main.js

==注意:路径上, 文件夹, 文件名不能叫webpack/其他已知的模块名==

1. 初始化项目目录和包环境=> 使用`git bash`操作

   <img src="assets/image-20210313115025631.png" align="left" alt="image-20210313115025631" style="zoom:50%;" />

   ```bash
   mkdir webpack-start
   cd webpack-start
   npm init -y
   ```

2. 安装依赖包

   ```bash
   npm i webpack webpack-cli -D
   ```

3. 配置scripts(自定义命令)

   ```bash
   scripts: {
   	"build": "webpack"
   }
   ```

4. 新建目录src

5. 新建src/add/add.js - 定义求和函数导出

   ```js
   export const addFn = (a, b) => a + b
   ```

6. 新建src/index.js导入使用

   ```js
   import {addFn} from './add/add'
   
   console.log(addFn(10, 20));
   ```

7. 运行打包命令

   ```bash
   npm run build # yarn build
   ```

> 总结: src并列处, 生成默认==dist==目录和打包后默认main.js文件

### 2.1_webpack 更新打包

> 目标: 以后代码变更, 如何重新打包呢

1. 新建src/tool/tool.js - 定义导出数组求和方法

   ```js
   export const getArrSum = arr => arr.reduce((sum, val) => sum += val, 0)
   reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。
   reduce() 可以作为一个高阶函数，用于函数的 compose。
   注意: reduce() 对于空数组是不会执行回调函数的。
   array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
   参数	描述
   function(total,currentValue, index,arr)	必需。用于执行每个数组元素的函数。
   函数参数:
   参数	描述
   total	必需。初始值, 或者计算结束后的返回值。
   currentValue	必需。当前元素
   currentIndex	可选。当前元素的索引
   arr	可选。当前元素所属的数组对象。
   initialValue	可选。传递给函数的初始值
   ```
   
2. src/index.js - 导入使用

   ```js
   import {addFn} from './add/add'
   import {getArrSum} from './tool/tool'
   
   console.log(addFn(10, 20));
   console.log(getArrSum([1, 2, 3]));
   ```
   
3. 重新打包

   ```bash
   npm run build
   会覆盖之前的
   ```

> 总结1: src下开发, dist是打包后代码
>
> 总结2: 打包后格式压缩, 变量压缩等

### 2.2_小结

> 目标: 熟悉webpack基础使用步骤

1. 准备yarn/npm软件

2. 初始化环境-package.json文件-自定义打包命令

3. 默认src/index.js打包入口文件, 直接/间接产生引用关系才会被打包 - 编写代码

4. 执行打包命令, 默认输出到dist/main.js

## 3. webpack的配置

### 3.0_webpack-入口和出口

> 目标: 告诉webpack从哪开始打包, 打包后输出到哪里

#### 入口和出口

默认入口: ./src/index.js

默认出口: ./dist/main.js

webpack配置 - webpack.config.js(默认)

1. 和src同级,新建 webpack.config.js
2. 填入配置项

```js
const path = require("path")

module.exports = {
    entry: "./src/main.js", // 入口
    output: { 
        path: path.join(__dirname, "dist"), // 出口路径
        filename: "bundle.js" // 出口文件名
    }
}
```

3. 修改package.json, 自定义打包命令 - 让webpack使用配置文件

```json
"scripts": {
    "build": "webpack --config webpack.config.js"
},
从此不要加 "type":"module"，因为webpack会帮我们转，加上反而冲突
```

4. 打包观察效果

### 扩展-打包流程和模式

#### 流程分析

![image-20210227101044654](assets/image-20210227101044654.png)

==重点: 所有要被打包的资源都要跟入口产生直接/间接的引用关系==

说明❓：`__dirname`指向项目根目录

#### 打包模式

```js
module.exports = {
  // 打包的模式 =》 'production'(生产环境打包)  | 'development'（开发环境打包）
  /**
   * 1. production => 打出来的代码会压缩和混淆代码 =》代码写完了，把mode的值改成prod打包，上传服务器
   * 2. development =》打出来的代码不会压缩和混淆 =》开发期间就用这个模式，打包速度快
   */
  // mode: 'development',
  mode: 'production' 
} 
```

### 3.1_案例-webpack隔行变色

> 目标: 工程化模块化开发前端项目, webpack会对ES6模块化处理

1. 准备环境

   * 初始化包环境

   * 下载依赖包
   * 配置自定义打包命令

2. 下载jquery, 新建public/index.html

   ```bash
   npm i jquery
   ```

   ![image-20210208100817930](assets/image-20210208100817930.png)

3. index.html 准备一些li, 

   * ==引入dist/bundle.js==

   * ==因为import语法浏览器支持性不好, 需要被webpack转换后, 再使用JS代码==

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <meta http-equiv="X-UA-Compatible" content="ie=edge">
     <title>Document</title>
   </head>
   <body>
   
   <div id="app">
     <!-- ul>li{我是第$个li}*10 -->
     <ul>
       <li>我是第1个li</li>
       <li>我是第2个li</li>
       <li>我是第3个li</li>
       <li>我是第4个li</li>
       <li>我是第5个li</li>
       <li>我是第6个li</li>
       <li>我是第7个li</li>
       <li>我是第8个li</li>
       <li>我是第9个li</li>
     </ul>
   </div>
   
   <script src="../dist/bundle.js"></script>
   </body>
   </html>
   ```

4. 在src/main.js引入jquery

   ```js
   // 引入jquery
   import $ from 'jquery'
   ```

5. src/main.js中编写[隔行变色](https://www.w3school.com.cn/jquery/jquery_ref_selectors.asp)代码

   ```js
   $(function() {
     $('#app li:nth-child(odd)').css('color', 'red') // 奇数
     $('#app li:nth-child(even)').css('color', 'green') // 偶数
   })
   ```

6. 执行打包命令观察效果

> 总结: 前端工程化模块化, 需要使用的第三方包代码在npm下, 被webpack打包后引入到html中使用

### 3.2_插件-自动生成html文件

> 目标: html-webpack-plugin插件, 让webpack打包后生成html文件并自动引入打包后的js

[html-webpack-plugin插件地址](https://www.webpackjs.com/plugins/html-webpack-plugin/)

  1. 下载插件

     ```
     npm i html-webpack-plugin  -D
     ```

  2. webpack.config.js配置 插件的优先级在js的优先级之下

     ```js
     // 引入自动生成 html 的插件
     const HtmlWebpackPlugin = require('html-webpack-plugin')
     
     module.exports = {
         // ...省略其他代码
         plugins: [
             new HtmlWebpackPlugin({
                 template: './public/index.html' // 以此为基准生成打包后html文件
             })
         ]
     }
     ```

  3. 删除public/index.html中 - 手动引入打包后js代码

     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <meta http-equiv="X-UA-Compatible" content="ie=edge">
       <title>Document</title>
     </head>
     <body>
     
     <div id="app">
       <!-- ul>li{我是第$个li}*10 -->
       <ul>
         <li>我是第1个li</li>
         <li>我是第2个li</li>
         <li>我是第3个li</li>
         <li>我是第4个li</li>
         <li>我是第5个li</li>
         <li>我是第6个li</li>
         <li>我是第7个li</li>
         <li>我是第8个li</li>
         <li>我是第9个li</li>
       </ul>
     </div>
     
     <!-- 打包后的文件会被自动引入, 不需要手动引入了 -->
     </body>
     </html>
     ```

4. 重新打包后观察dist下是否多出html并运行看效果

> 总结: webpack就像一个人, webpack.config.js是人物属性, 给它穿什么装备它就干什么活

### 3.3_加载器 - 处理css文件 

> 目标: loaders加载器, 可让webpack处理其他类型的文件, 打包到js中

原因❓: webpack默认只认识 js 和 json文件

[style-loader文档](https://webpack.docschina.org/loaders/style-loader/)

[css-loader文档](https://webpack.docschina.org/loaders/css-loader/)

1. 安装依赖

   ```
   npm i style-loader css-loader -D
   ```

2. webpack.config.js 配置

   ```js
   module.exports = {
       // ...其他代码
       module: { 
           rules: [ // loader的规则
             {
               test: /\.css$/, // 匹配所有的css文件
               // use数组里从右向左运行
               // 先用 css-loader 让webpack能够识别 css 文件的内容并打包
               // 再用 style-loader 将样式, 把css插入到dom中
               use: [ "style-loader", "css-loader"]
             }
           ]
       }
   }
   ```
   
3. 新建src/css/li.css - 去掉li默认样式

   ```css
   ul, li{
       list-style: none;
   }
   ```

4. 引入到main.js (因为这里是入口需要产生关系, 才会被webpack找到打包起来)

   ```js
   import "./css/index.css"
   ```

5. 运行打包后dist/index.html观察效果和css引入情况

> 总结: 万物皆模块, 引到入口, 才会被webpack打包, css打包进js中, 然后被嵌入在style标签插入dom上

### 3.4_加载器 - 处理less文件

> 目标: less-loader让webpack处理less文件, less模块翻译less代码

[less-loader文档](https://webpack.docschina.org/loaders/less-loader/)

1. 下载依赖包

   ```bash
   npm i less less-loader -D
   ```
2. webpack.config.js 配置

   ```js
   module: {
     rules: [ // loader的规则
       // ...省略其他
       {
       	test: /\.less$/,
       	// 使用less-loader, 让webpack处理less文件, 内置还会用less翻译less代码成css内容
           use: [ "style-loader", "css-loader", 'less-loader']
       }
     ]
   }
   ```
   
3. src/less/index.less  - 设置li字体大小24px

   ```less
   @size:24px;
   
   ul, li{
       font-size: @size;
   }
   ```

4. 引入到main.js中

   ```js
   import "./less/index.less"
   ```

5. 打包运行dist/index.html 观察效果

> 总结: 只要找到对应的loader加载器, 就能让webpack处理不同类型文件

### 3.5_加载器 - 处理图片文件

文件请求

> 目标: 用url-loader和file-loader, webpack支持文件打包处理

[url-loader文档](https://webpack.docschina.org/loaders/url-loader/)

[file-loader文档](https://webpack.docschina.org/loaders/file-loader/)

1. 下载依赖包

   ```bash
   npm i url-loader file-loader -D
   
   file-loader的作用
   
   1、作用：file-loader可以用来帮助webpack打包处理一系列的图片文件；比如：.png 、 .jpg 、.jepg等格式的图片；
   
   2、使用file-loader打包图片的结果：使用file-loader打包的图片会给每张图片都生成一个随机的hash值作为图片的名字；
   简答地说,url-loader封装了file-loader。url-loader不依赖于file-loader,即使用url-loader时,只需要安装url-loader即可,不需要安装file-loader,因为url-loader内置了file-loader
   url-loader 允许你有条件地将文件转换为内联的 base-64 URL (当文件小于给定的阈值)，这会减少小文件的 HTTP 请求数。如果文件大于该阈值，会自动的交给 file-loader 处理。
   ```

2. webpack.config.js 配置

   ```js
   老版本的webpack4.0不能转为base64
   {
     test: /\.(png|jpg|gif|jpeg)$/i,
     use: [
       {
         loader: 'url-loader', // 匹配文件, 尝试转base64字符串打包到js中
         // 配置limit, 超过8k, 不转, file-loader复制, 随机名, 输出文件
         options: {
           limit: 8 * 1024,
         },
       },
     ],
         新版要用parser配置对象代替use 
     type:'asset',
     parser:{
         dataUrlCondition:{
             maxSize: 8 * 1024 //8kb
         }
     }
   }
     
     
   ```

   图片转成 base64 字符串

   - 好处就是浏览器不用发请求了，直接可以读取
   - 坏处就是如果图片太大，再转`base64`就会让图片的体积增大 30% 左右

3. src/assets/准备老师发的2个图文件

4. 在css/less/index.less - 把小图片用做背景图

   ```less
   body{
       background: url(../assets/small.png) no-repeat center;
   }
   ```

5. 在src/main.js - 把大图插入到创建的img标签上, 添加body上显示

   ```js
   // 引入图片-使用
   import imgUrl from './assets/big.gif'
   const theImg = document.createElement("img")
   theImg.src = imgUrl
   document.body.appendChild(theImg)
   ```

6. 打包运行dist/index.html观察2个图片区别

> 总结: 根据配置文件 url-loader 把文件转base64 打包进js； file-loader 把文件直接复制输出

### 3.6_加载器 - 处理字体文件

> 目标: 用url-loader和file-loader处理, 字体图标文件

1. webpack.config.js - 准备配置

   ```js
    { // 处理字体图标的解析
        test: /\.(eot|svg|ttf|woff|woff2)$/,
            use: [
                {
                    loader: 'url-loader',
                    options: {
                        limit: 2 * 1024,
                        // 配置输出的文件名   []  表示站位符 
                        name: '[name].[ext]',
                        // 配置输出的文件目录
                        outputPath: "fonts/"
                    }
                }
            ]
    }
   ```
说明❓：`[name]`占位符，自动读取资源名称命名；例如：[hash]占位生成随机hash值作为名称一部分
2. src/assets/ - 放入字体库fonts文件夹

3. 在main.js引入iconfont.css

   ```js
   // 引入字体图标文件
   import './assets/fonts/iconfont.css'
   ```

4. 在public/index.html使用字体图标样式

   ```html
   <i class="iconfont icon-weixin"></i>
   ```

5. 执行打包命令-观察打包后网页效果

> 总结: url-loader和file-loader 可以打包静态资源文件

### 3.7 加载器 - 处理高版本js语法

> 目标: 让webpack对高版本 的js代码, 降级处理后打包

写代码演示: 高版本的js代码(箭头函数), 打包后, 直接原封不动打入了js文件中, 遇到一些低版本的浏览器就会报错

原因: **webpack 默认仅内置了 模块化的 兼容性处理**   `import  export`

babel 的介绍 => 用于处理高版本 js语法 的兼容性  [babel官网](https://www.babeljs.cn/)

解决: 让webpack配合babel-loader 对js语法做处理

[babel-loader文档](https://webpack.docschina.org/loaders/babel-loader/)

  1. 安装包

     ```bash
     npm i -D babel-loader @babel/core @babel/preset-env
     ```

  2. 配置规则

     ```js
     module: {
       rules: [
         {
             test: /\.js$/,
             exclude: /(node_modules|bower_components)/,
             use: {
                 loader: 'babel-loader',
                 options: {
                     presets: ['@babel/preset-env'] // 预设:转码规则(用bable开发环境本来预设的)
                 }
             }
         }
       ]
     }
     ```

3. 在main.js中使用箭头函数(高版本js)

   ```js
   // 高级语法
   const fn = () => {
     console.log("你好babel");
   }
   console.log(fn) // 这里必须打印不能调用/不使用, 不然webpack会精简成一句打印不要函数了/不会编译未使用的代码
   // 没有babel集成时, 原样直接打包进lib/bundle.js
   // 有babel集成时, 会翻译成普通函数打包进lib/bundle.js
   ```

4. 打包后观察lib/bundle.js - 被转成成普通函数使用了 - 这就是babel降级翻译的功能

> 总结: babel-loader 可以让webpack 对高版本js语法做降级处理后打包

### 3.8 小结

Webpack作用：打包前端静态文件=》默认只能打包js和json

加载器 - 让webpack识别不同类型文件 - 提取文件里代码

插件 - 让webpack拥有更多功能

多去官网文档寻找 插件/加载器 名字和使用方式

**口诀**: 找模块, 下模块, 配置模块, 打包使用

## 4. webpack 开发服务器

抛出问题: 每次修改代码, 都需要重新` npm run build` 打包, 才能看到最新的效果, 实际工作中, 打包=> 非常费时 (30s - 60s) 之间

为什么费时? 

1. 构建依赖
2. 磁盘读取对应的文件到内存, 才能加载  
3. 用对应的 loader 进行处理  
4. 将处理完的内容, 输出到磁盘指定目录  

解决问题: 起一个开发服务器,  在电脑内存中打包, 缓存一些已经打包过的内容, 只重新打包修改的文件, 最终运行加载在内存中给浏览器使用

### 4.0_webpack-dev-server自动打包刷新

> 目标: 启动本地服务, 可实时更新修改的代码, 打包**变化代码**到内存中, 然后直接提供端口和网页访问

说明：新建`webpack.dev.js`测试

1. 下载包

   ```bash
   npm i webpack-dev-server -D
   ```

2. 配置自定义命令

   ```js
   scripts: {
   	"build": "webpack --config webpack.config.js",
   	"serve": "webpack serve --config webpack.dev.js"
   }
   ```

3. 在webpack.dev.js中添加服务器配置

   ```js
   module.exports = {
       // ...其他配置
       devServer: {
         port: 3000, // 端口号
         open: true // 自动打开浏览器
       }
   }
   ```

4. 运行命令-启动webpack开发服务器

   ```bash
   npm run serve 
   #或者 yarn serve
   ```

> 总结: 以后改了src下的资源代码, 就会直接自动打包更新到内存, 然后浏览器自动刷新

server打包后存储到了内存中，没有存储在硬盘，内存读取速度快，看不到dist，如果改了服务器底层就是webpack的配置文件要重启服务器

### 4.1_webpack下source map了解和使用

关于环境：

1. 开发环境 =》开发的时候，本地使用live-server或webpack-dev-server运行代码的服务器
2. 生产环境（灰度环境(过渡的环境)=》过渡）=》带域名和服务器正式的完整代码运行服务器

> 目标: source map概念, 用于在浏览器调试错误使用, 记录代码打包前原始位置

说明：可在`webpack.dev.js`中测试

准备: src/main.js产生一个报错, 启动webpack服务器

```js
console.log(abc); // 不要声明abc变量
```

问题❓：错误的行号和源代码的行号不一致=》影响错误调试

![image-20210603164411665](assets/image-20210603164411665.png)

> #### 解决方案: 启用source map（代码地图）

#### 开发环境

1. webpack.config.js - 配置

   ```js
   module.exports = {
     // ...其他配置
     mode: 'development', // 开发模式 - webpack内部会使用内置优化
     devtool: 'cheap-module-source-map',（推荐） // cheap-module-source-map 开发模式下使用, 保证运行时的行数 和 源代码行数 一致 
     // devtool: 'eval-cheap-source-map', // 运行时的行数 和 源代码行数 不一致（构建速度快）
   }
   ```

2. 观察是否有错误代码打包前的位置信息了

注意：修改配置文件需要重新启动执行`npm run serve`

#### 生产环境  压缩

1. 不显示源码=>显示报错行数（**推荐**）

   ```js
   mode: 'production',
   devtool: 'nosources-source-map'
   ```

   ![image-20201213135929321](assets/image-20201213135929321.png)

2. 显示源码和行数

   ```js
   mode: 'production',
   devtool: 'source-map'
   ```


![image-20201213140218039](assets/image-20201213140218039.png)



#### devtool常用组合  




![image-20210227130825907](assets/image-20210227130825907.png)

规则字符串列表:https://webpack.docschina.org/configuration/devtool/

### 4.2 小结

1. webpack-dev-server 可以加快webpack打包速度, 只重新打包更新的代码

2. webpack-dev-server 打包的代码在内存中, 比磁盘位置更快

3. source map 用于记录代码信息，方便调试错误

## 今日总结

- [ ] 熟悉npm使用
- [ ] 什么是webpack, 它有什么作用=》定义：打包静态模块（ .js .html .css .less .png等）的工具（node）  
- [ ] webpack让模块化开发前端项目成为了可能, 底层需要**node支持**
- [ ] webpack基本使用步骤 =>  1. 新建项目目录 2. 目录下安装webpack   3. 默认直接可以打包
- [ ] 了解webpack各种配置项=》自定义（增强）打包
  - [ ] 入口(entry)/出口(output)
  - [ ] 插件(plugin) =》增强webpack功能
  - [ ] 加载器(loader) => 让webpack支持打包更多文件类型
  - [ ] mode模式 =》设置开发（打包速度快）和生成模式打包（压缩混淆）
  - [ ] devServer =》1. 运行一个本地开发服务器 2. 自动打包修改的代码和自动刷新
- [ ] source map的作用 => 调试代码错误



## 今日作业

可以调研如何把css提取成一个独立的文件打包后=> 提示❓：搜索插件`MiniCssExtractPlugin`用法

