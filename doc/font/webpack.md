# webpack

## Webpack 中文文档

[webpack 中文文档](https://webpack.docschina.org/concepts/)

### 1. 概念

本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，然后将你项目中所需的每一个模块组合成一个或多个 *bundles*，它们均为静态资源，用于展示你的内容。

在开始前你需要先理解一些**核心概念**：

- 入口（entry）：指示 webpack 应该使用哪个模块
- 输出（output）：告诉 webpack 在哪里输出它所创建的 bundle，以及如何命名这些文件
- loader：webpack 只能理解 JavaScript 和 JSON 文件，loader 让 webpack 能够去处理其他类型的文件 
- 插件（plugin）：执行范围更广的任务，包括打包优化、资源管理、注入环境变量
- 模式（mode）：相应值的优化，默认值是 production
- 浏览器兼容性（browser compatibility）：webpack 支持所有符合 ES5 标准的浏览器
- 环境（environment）：webpack 5 运行于 Node.js v10.13.0+ 的版本

### 2. 入口起点（entry）

1. 单个入口语法

```js
module.exports = {
  entry: './test.js'
}
```

2. 对象语法

```js
module.exports = {
  entry: {
    app: './src/app.js',
    test: './src/test/test.js'
  }
}
```

3. 用于描述入口的对象：

- dependOn： 当前入口所依赖的入口，它们必须在该入口被加载前加载
- filename：指定要输出的文件名称
- import：启动时需要加载的模块
- library：为当前的 entry 构建一个 library
- runtime：运行时 chunk 的名字
- publicPath：当该入口的输出文件在浏览器中被引用时，为它们指定一个公共 URL 地址

### 3. 输出（output）

可以通过配置 output 选项，告知 webpack 如何向硬盘写入编译文件，即使可以存在多个 entry 起点，但只能指定一个 output 配置

1. 在 webpack 中，output 属性的最低要求是，将它的值设置为一个对象，然后为将输出文件的文件名配置为一个 output.filename

```js
module.exports = {
  output: {
    filename: 'bundle.js'
  }
}
```

此配置将一个单独的 bundle.js 文件输出到 dist 目录中

2. 多个入口起点的情况，应该使用占位符来确保每个文件具有唯一的名称

```js
module。exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
```

### 4. loader

loader 用于对模块的源代码进行转换。loader 可以使你在 import 或 "load(加载)" 模块时预处理文件

1. 示例

- 首先安装相关的 loader：

```bash
npm install --save-dev scss-loader ts-loader
```

- 然后指示 webpack 对每个 .css 使用 css-loader，以及对所有 .ts 文件使用 ts-loader:

```js
module.exports = {
  module: {
    rules: [
      {test: /\.scss$/, use: 'scss-loader'},
      {test: /\.ts$/, ues: 'ts-loader'}
    ]
  }
}
```

2. loader 特性

- 支持链式调用
- 可以是同步，也可以是异步
- 运行在 Node.js 中，并且能够执行任何操作
- 插件带来了更多的特性

### 5. plugin

插件的目的在于解决 loader 无法实现的其他事。webpack 插件是一个具有 apply 方法的 JavaScript 对象。apply 方法会被 webpack compiler 调用，并且在整个编译生命周期都可以访问 compiler 对象

1. 配置方式

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack'); // 访问内置的插件
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader',
      },
    ],
  },
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: './src/index.html' }),
  ],
};
```

2. Node API 方式

```js
const webpack = require('webpack'); // 访问 webpack 运行时(runtime)
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration);

new webpack.ProgressPlugin().apply(compiler);

compiler.run(function (err, stats) {
  // ...
});
```

### 6. 配置（Configuration)

由于 webpack 遵循 CommonJS 模块规范，因此可以在配置中使用：

- 通过 require(...) 引入其它文件
- 通过 require(...) 使用 npm 下载的工具函数
- 使用 ?: 操作符
- 对 value 使用常量或变量赋值
- 编写并执行函数，生成部分配置

1. 基本配置

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js',
  },
};
```

### 7. 模块（Modules）

在模块化编程中，开发者将程序分解为功能离散的 chunk，并称之为模块。每个模块都拥有小于完整程序的体积，使得验证、调试及测试变得轻而易举。精心编写的模块提供了可靠的抽象和封装边界，使得引用程序中的每个模块都具备了条理清晰的设计和明确的目的。Node.js 从一开始就支持模块化编程。

webpack 模块能以各种方式表达它们的依赖关系：

- ES 2015 import 语句
- Common require() 语句
- AMD define 和 require 语句
- css/sass/less 文件中的 @import 语句
- stylesheet url(...) 或者 HTML `<img src=...>` 文件中的图片链接

### 8. 模块解析（Module Resolution）

resolver 是一个帮助寻找模块绝对路径的库。一个模块可以作为另一个模块的依赖模块，然后被后者引用。resolver 帮助 webpack 从每个 require/import 语句中找到需要引入到 bundle 中的模块代码

webpack 能解析三种文件路径，解析规则：

1. 绝对路径

```js
import '/home/me/file'
```

2. 相对路径

```js
import '../src/file1';
import './file2';
```

3. 模块路径

```js
import 'module'
import 'module/lib/file'
```

### 9. Module Federation

多个独立的构建可以组成一个应用程序，这些独立的构建之间不应该存在依赖关系，因此可以单独开发和部署它们（这通常称为微前端，但是不仅如此）

### ，，，

## 深入浅出 Webpack

[深入浅出 Webpack](https://webpack.wuhaolin.cn/)

不得不说，写的是真的好！

### 1. 入门

### 1.1. 前端的发展

近年来 Web 应用变得更加复杂和庞大，Web 前端技术的应用也更加广泛。从复杂庞大的管理后台到对性能要求苛刻的移动网页，再到类似 ReactNative 的原生应用开发方案，Web 前端工程师再面临更多机遇的同时也会面临更大的挑战

#### 1.1.1. 模块化

模块化是指把一个复杂的系统分解到多个模块一方便编码。很久以前通过命名空间来组织代码的方式有一些问题：

- 命名空间冲突，两个库可能会使用同一个名称
- 无法合理地管理项目的依赖和版本
- 无法方便地控制依赖的加载顺序

#### 1.1.2. CommonJS

CommonJS 是一种使用广泛的 JavaScript 模块化规范，核心思想是通过 require 方法来同步地加载依赖的其它模块，通过 module.exports 导出需要暴露的接口

```js
// 导入
const moduleA = require('./moduleA')
// 导出
module.exports = moduleA.someFunc
```

CommonJS 的优点是：

- 代码可复用于 Node.js 环境下并运行
- 通过 NPM 发布的很多第三方模块都采用了 CommonJS 规范

#### 1.1.3.AMD

[AMD](https://en.wikipedia.org/wiki/Asynchronous_module_definition) 也是一种 JavaScript 模块化规范，与 CommonJS 最大的不同在于它采用异步的方式去加载依赖的模块。 AMD 规范主要是为了解决针对浏览器环境的模块化问题，最具代表性的实现是 [requirejs](http://requirejs.org/)

```js
// 定义一个模块
define('module', ['dep'], function(dep) {
  return exports;
})
// 导入和使用
require(['module'], function(module) {})
```

AMD 的优点在于：

- 可在不转换代码的情况下直接在浏览器中运行
- 可异步加载依赖
- 可并行加载多个依赖
- 代码可运行在浏览器环境和 Node.js 环境下

AMD 的缺点在于JavaScript 运行环境没有原生支持 AMD，需要先导入实现了 AMD 的库后才能正常使用

#### 1.1.4. ES6 模块化

它将逐步取代 CommonJS 和 AMD 规范，称为浏览器和服务器通用的模块解决方案

```js
// 导入
import{ readFile } from 'fs'
import React from 'react'
// 导出
export function hello() {}
export default {
  // ...
}
```

ES6 虽然是未来的模块化方案，但是有些浏览器环境不支持 ES6，必须通过工具转换成标准的 ES6 后才能运行

#### 1.1.5 样式文件中的模块化

处理 JavaScript 开始模块化改造，前端开发里的样式文件也支持模块化。以 SCSS 为例，把一些常用的样式片段放进一个通用的文件里，再在另一个文件里通过 @import 语句去导入和使用这些样式片段

```scss
// util.scss
@mixin center {
  // 水平垂直居中
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%)
}
// main.scss
@import 'util';
#box {
  @include center;
}
```

### 1.2 常见的构建工具及对比

前端技术发展之快，各种可以提高开发效率的新思想和框架被发明。但是这些东西都有一个共同点：源代码无法直接运行，必须通过转换后才可以正常运行。

构建就是做这件事情，把源代码转换成发布到线上的可执行 JavaScrip、CSS、HTML 代码，包括如下内容：

- 代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等
- 文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等
- 代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
- 模块合并：在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件
- 自动刷新：监听本地源代码的变化、自动重新构建、刷新浏览器
- 代码校验：在代码被提交到仓库前需要检验代码是否符合规范，以及单元检测是否通过
- 自动发布：更新完成代码后，自动构建出线上发布代码并传输给发布系统

构建其实是在工程化、自动化思想在前端开发中的体现，把一系列流程用代码去实现，让代码自动化地执行这一系列复杂的流程。构建给前端开发注入了更大的活力，解放了我们的生产力

#### 1.2.1. npm Script

npm Script 是一个任务执行者，是 npm 内置的一个功能，允许在 package.json 文件中使用 scripts 字段定义任务：

```json
{
  'scripts': {
    'dev': 'node dev.js',
    'pub': 'node build.js'
  }
}
```

Npm Script的优点是内置，无须安装其他依赖。其缺点是功能太简单，虽然提供了 `pre` 和 `post` 两个钩子，但不能方便地管理多个任务之间的依赖。

#### 1.2.2. Grunt

Grunt 相当于进化版的 Npm Script，有大量现成的插件封装了常见的任务，也能管理任务之间的依赖关系，自动化执行依赖的任务，每个任务的具体执行代码和依赖关系写在配置文件 `Gruntfile.js` 里，但是集成度不高，需要些很多配置之后才可以用，无法做到开箱即用

#### 1.2.3. Gulp

[Gulp](http://gulpjs.com/) 是一个基于流的自动化构建工具。 除了可以管理和执行任务，还支持监听文件、读写文件。Gulp 被设计得非常简单，只通过下面5个方法就可以胜任几乎所有构建场景：

- 通过 `gulp.task` 注册一个任务；
- 通过 `gulp.run` 执行任务；
- 通过 `gulp.watch` 监听文件变化；
- 通过 `gulp.src` 读取文件；
- 通过 `gulp.dest` 写文件。

Gulp 的最大特点是引入了流的概念，同时提供了一系列常用的插件去处理流，流可以在插件之间传递。Gulp 的优点是好用又不失灵活，既可以单独完成构建也可以和其它工具搭配使用。其缺点是和 Grunt 类似，集成度不高，要写很多配置后才可以用，无法做到开箱即用。

可以将Gulp 看作 Grunt 的加强版。相对于 Grunt，Gulp增加了监听文件、读写文件、流式处理的功能。

#### 1.2.4. Webpack

Webpack 是一个打包模块化 JavaScript 的工具，在 Webpack 里一切文件皆模块，通过 Loader 转换文件，通过 Plugin 注入钩子，最后输出由多个模块组合成的文件。Webpack 专注于构建模块化项目：

![图1-2 Webpack 简介](../image/1-2webpack.png)

Webpack 的优点是：

- 专注于处理模块化项目，能做到开箱即用
- 通过 Plugin 扩展，完整好用又不失灵活（sass-loader)
- 使用常见不仅限于 Web 开发
- 社区活跃，开发体验良好

Webpack 的缺点是只能用于采用模块化开发的项目

#### 1.2.5. 为什么选择 Webpack

上面介绍的构建工具是按照它们诞生的时间排序的，它们是时代的产物，侧面反映出 Web 开发的发展趋势如下：

1. 在 Npm Script 和 Grunt 时代，Web 开发要做的事情变多，流程复杂，自动化思想被引入，用于简化流程；
2. 在 Gulp 时代开始出现一些新语言用于提高开发效率，流式处理思想的出现是为了简化文件转换的流程，例如将 ES6 转换成 ES5。
3. 在 Webpack 时代由于单页应用的流行，一个网页的功能和实现代码变得庞大，Web 开发向模块化改进。

### 1.3 安装 Webpack

还是得自己实践一下，这种事就得 `show your code`

1. `npm init`，`npm i -D webpack`

2. ```
   |-- index.html
   |-- main.js
   |-- show.js
   |-- webpack.config.js
   ```

```html
<html>
<head>
  <meta charset="UTF-8">
</head>
<body>
<div id="app"></div>
<!--导入 Webpack 输出的 JavaScript 文件-->
<script src="./dist/bundle.js"></script>
</body>
</html>
```

```js
// main.js 这里是 commonJS 规范
const show = require('./show.js');
// 执行 show 函数
show('Webpack');
```

```js
// show.js
// 操作 DOM 元素，把 content 显示到网页上
export function show(content) {
  window.document.getElementById('app').innerText = 'Hello,' + content;
}
```

```js
// webpack.config.js
const path = require('path')
module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
```

3. `npm run build` ，多了一个build 文件夹，里面有一个 bunddle.js 文件，打开html 就可以看到运行结果

Webpack 是一个打包模块化 JavaScript 的工具，它会从 main.js （入口文件）出发，识别出源码中的模块化导入语句，递归的寻找出入口文件的所有依赖，把入口和其所有依赖打包到一个单独的文件中。从 Webpack 2 开始，已经内置了对 ES6、CommonJS、AMD 模块化语句的支持

### 1.4. 使用 loader

安装 css-loader  style-loader 可能会失败，需要指定版本

1. 添加一个 css 文件并在项目中使用
2. 安装相应 loader，添加这段代码进入 webpack.config.js，use 从右到左执行，minimize  是压缩文件，style-load 把 CSS 内容注入到 JavaScript 里

```js
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader?minimize']
      }
    ]
  }
```

能明显发现 build.js 的代码内容变多了，且出现了 css 内容，style-loader 将 CSS 内容用 JavaScript 的字符串存储起来，在网页执行 JavaScript 时通过 DOM 操作动态地王 HTML head 标签插入 HTML style 标签。这样做可能会导致 JavaScript 变大并导致加载网页时间变长，想让 Webpack 单独输出 css 文件，可以通过 Plugin 机制实现

### 1.5 使用 Plugin

Plugin 是用来扩展 Webpack 功能的，通过在构建流程里注入钩子实现，它给 Webpack 带来了很大的灵活性，修改如下：

```js
const path = require('path');
const ExtractTextPlugin = require('extract-text-webpack-plugin')

module.exports = {
  // JS 执行入口文件
  entry: './main.js',
  output: {
    // 把所有依赖的模块合并输出到一个 bundle.js 文件
    filename: 'bundle.js',
    // 输出文件都放到 dist 目录下
    path: path.resolve(__dirname, './dist'),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          // 转换 .css 文件需要使用的 Loader
          use: ['css-loader']
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin({
      filename: `[name]_[contenthash:8].css`
    })
  ]
};

```

但是这么些得自己重新引入 css 文件了

### 1.6 使用 DevServer

前面几节只是让 Webpack 正常运行起来了，但是在实际开发中，你可能会需要：

1. 提供 HTTP 服务而不是使用本地文件预览
2. 监听文件的变化并自动刷新网页，做到实时预览
3. 支持 Source Map，以方便调试

Webpack 原生支持上述2，3点，再结合官方提供的工具 DevServer 也可以方便地做到第 1 点。DevServer 会启动一个 HTTP 服务器用于服务器网页请求，同时会帮助启动 Webpack，并接收 Webpack 发出的文件变更信号，通过 WebSocket 协议自动刷新网页做到实时预览

使用 DevServer 之后我没有该 js 文件路径也可以运行，改了之后也可以运行，应该是缓存和本地的文件都读到了？

#### 1.6.1. 实时预览

Webpack 在启动时可以开启监听模式，开启监听模式后 Webpack 会监听本地文件系统的变化，发生变化时重新构建出新的结果。启动 Webpack 时可以通过 `webapck --watch` 来开启监听模式。DevServer会让 Webpack 在构建出的 JavaScript 代码里注入一个代理客户端用于控制网页，网页和 DevServer 之间通过 WebSocket 协议通信。

#### 1.6.2. 模块热替换

除了通过重新刷新整个网页来实现实时预览，DevServer 还有一种被称作模块热替换的刷新技术。 模块热替换能做到在不重新加载整个网页的情况下，通过将被更新过的模块替换老的模块，再重新执行一次来实现实时预览。

#### 1.6.3. 支持 Source Map

在浏览器中运行的 JavaScript 代码都是编译器输出的代码，这些代码的可读性很差。如果在开发过程中遇到一个不知道原因的 Bug，则你可能需要通过断点调试去找出问题。 在编译器输出的代码上进行断点调试是一件辛苦和不优雅的事情， 调试工具可以通过 [Source Map](https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/) 映射代码，让你在源代码上断点调试。 Webpack 支持生成 Source Map，只需在启动时带上 `--devtool source-map` 参数。 加上参数重启 DevServer 后刷新页面，再打开 Chrome 浏览器的开发者工具，就可在 Sources 栏中看到可调试的源代码了。

### 1.7 核心概念

Webpack 的核心概念：

- Entry：入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象从输入
- Module：模块，在 Webpack 中一切皆模块，一个模块对应着一个文件
- Chunk：代码块，一个 Chunk 由多个模块组合而成，用于代码合并和分割
- Loader：模块转换器，用于把模块原内容按照需求转换成新内容
- Plugin：扩展插件，在 Webpack 构建流程中注入扩展逻辑来改变扩展结果
- Ouptput：输出结果，经过 Webpack 一系列处理得到的最终结果

Webpack 启动后会从 Entry 里配置的 Module 开始递归解析 Entry 依赖的所有 Module。 每找到一个 Module， 就会根据配置的 Loader 去找出对应的转换规则，对 Module 进行转换后，再解析出当前 Module 依赖的 Module。 这些模块会以 Entry 为单位进行分组，一个 Entry 和其所有依赖的 Module 被分到一个组也就是一个 Chunk。最后 Webpack 会把所有 Chunk 转换成文件输出。 在整个流程中 Webpack 会在恰当的时机执行 Plugin 里定义的逻辑。

## 2.配置

配置 Webpack 的方式有两种：

1. 通过一个 JavaScript 文件描述配置，例如使用 webpack.config.js 文件里的配置
2. 通过命令行参数传入，例如 `webpack --devtool source-map`

### 2.1. Entry 

entry 是配置模块的入口，可抽象成输入，Webpack 执行构建的第一步将从入口开始搜寻及递归解析所有入口依赖的模块

#### 2.1.1. context

Webpack 在寻找相对路径的文件时会以 context 为根目录，context 默认为执行启动 Webpack 所在的当前工作目录。如果想改变 context 的默认配置，则可以在配置文件中这样设置它：

```js
module.exports = {
  context: path.resolve(__dirname, 'app')
}
```

context 必须是一个绝对路径的字符串

#### 2.1.2. Entry 类型

Entry 类型可以是以下三种中的一种或者相互组合：

- string：`'./app/entry'` 入口模块的文件路径，可以是相对路径
- array：`['./app/entry1', './app/entry2']`  入口模块的文件路径，可以是相对路径
- object：`{a: './app/entry-a', b: './app/entry-b'}` 配置多个入口，每个入口生成一个 chunk

#### 2.1.3. chunk 名称

Webpack 会为每个生成的 chunk 取一个名称，Chunk 的名称和 Entry 的配置有关：

- 如果是 entry 是一个 string 或 array，就只会生成一个 Chunk，这时候 Chunk 的名称是 main
- 如果entry 是一个 Object，就可能会出现多个 Chunk，这时候 Chunk 的名称是 Object 里面的键值的名称

#### 2.1.4. 配置动态 Entry

假如项目里有多个页面需要为每个页面的入口配置一个 Entry，但这些页面的数量可能会不断增长，这时候 Entry 的配置会受到其它因素的影响导致不能写成静态的值，所以需要设置成一个函数动态的放回上面的配置：

```js
// 同步函数
entry: () => {
  return {
    a:'./pages/a',
    b:'./pages/b',
  }
};
// 异步函数
entry: () => {
  return new Promise((resolve)=>{
    resolve({
       a:'./pages/a',
       b:'./pages/b',
    });
  });
};
```

#### 2.1.5. ,,,



## 3. 实战

如何用 Webpack 去解决世纪项目中常见的场景，按照不同的场景划分成以下几类

1. 使用新语言：ES6、TypeScript、Flow、SCSS、PostCSS
2. 使用新框架：React、Vue、Angular
3. 单页应用：为单页应用生成 HTML、管理多个单页应用
4. 不同环境的项目：构建同构应用、构建 ELectron 应用、构建 Npm 模块、构建离线应用
5. 搭配其它工具使用：Npm Script、检查代码、通过 Node.js API 启动 Webpack、使用 Webpack Dev  Middleware
6. 加载特殊类型的资源：加载图片、加载 SVG、加载 Source Map

### 3.1. 使用 ES6 语言

虽然目前部分浏览器和 Node.js 已经支持 ES6，但由于它们对 ES6 所有的标准支持不全，这导致在开发中不敢全面地使用 ES6，所以需要把ES6 编写的代码转换成目前已经支持良好的 ES5 代码，这包含 2 件事：

1. 把新的 ES6 语法用 ES5 实现，例如 ES6 的 class 用 ES5 的 prototype 实现
2. 给新的 API 注入 polyfill，例如项目使用 fetch API 时，只有注入对应的 polyfill 后才能在低版本浏览器中正常运行

#### 3.1.1. Babel

Babel 可以方便的完成以上两件事，Babel 是一个 JavaScript 编译器，能将 ES6 代码转为 ES5 代码。在 Babel 执行编译的过程中，会从项目根目录下的 .babelrc 文件读取配置。.babelrc 是一个 JSON 格式的文件，内容大致如下：

```json
{
  "plugins": [
    [
      "transform-runtime",
      {
        "polyfill": false
      }
    ]
   ],
  "presets": [
    [
      "es2015",
      {
        "modules": false
      }
    ],
    "stage-2",
    "react"
  ]
}
```

plugins 属性告诉 Babel 要使用哪些插件，插件可以控制如何转换代码

presets 属性告诉 Babel 要转换的源码使用了哪些新的语法特性

### 3.2. 使用 TypeScript 语言

TypeScript 是 JavaScript 的一个超集，主要提供了类型检查系统和对 ES6 语法的支持，但不支持新的 API，目前没有任何环境支持原生的 TypeScript  代码，必须通过构建把它转换成 JavaScript 代码后才能运行

TypeScript 官方提供了能把 TypeScript 转换成 JavaScript 的编译器，你需要在当前项目根目录下新建一个用于配置编译选项的 tsconfig.json 文件，编译器默认会读取和使用这个文件，配置内容大致如下：

```json
{
  "compilerOptions": {
    "module": "commonjs", // 编译出的代码采用的模块规范
    "target": "es5", // 编译出的代码采用 ES 的哪个版本
    "sourceMap": true // 输出 Source Map 方便调试
  },
  "exclude": [ // 不编译这些目录里的文件
    "node_modules"
  ]
}
```

通过 npm install -g typescript 安装编译器到全局后，可以哦通过 tsc hello.ts 命令编译出 hello.js 和 hello.js.map 文件

#### 3.2.1 集成 Webpack

要让 Webpack 支持 TypeScript，需要解决以下 2 个问题：

1. 通过 Loader 把 TypeScript 转换成 JavaScript
2. Webpack 在寻找模块对应的文件时需要尝试 ts 后缀

相关配置：

```js
const path = require('path');

module.exports = {
  // 执行入口文件
  entry: './main',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist'),
  },
  resolve: {
    // 先尝试 ts 后缀的 TypeScript 源码文件
    extensions: ['.ts', '.js']
  },
  module: {
    rules: [
      {
        test: /\.ts$/,
        loader: 'awesome-typescript-loader'
      }
    ]
  },
  devtool: 'source-map',// 输出 Source Map 方便在浏览器里调试 TypeScript 代码
};
```

### 3.3. 使用 React 框架

其中 JSX 语法是无法在任何现有的 JavaScript 引擎中运行的，所以在构建过程中需要把源码转换成可以运行的代码，例如：

```jsx
// 原 JSX 语法代码
return <h1>hello, world</h1>
```

```js
// 转换成正常的 JavaScript 代码
return React.createElement('h1', null, 'Hello, world')
```

目前 Babel 和 TypeScript 都提供了对 React 语法的支持，下面分别来介绍如何在使用 Babel 或 TypeScript 的项目中接入 React 框架

#### 3.3.1 React 与 Babel

要在使用 Babel 的项目中接入 React 框架，只需要加入 React 所依赖的 Presets babel-preset-react，安装完依赖后，再修改 .babelrc 配置文件加入 React Presets：

```json
"presets": ["react"]
```

#### 3.3.2. React 与 TypeScript

TypeScript 相比于 Babel 的优点在于它原生支持 JSX 语法，你不需要重新安装新的依赖，只需修改一行配置。 但 TypeScript 的不同在于：

- 使用了 JSX 语法的文件后缀必须是 `tsx`。
- 由于 React 不是采用 TypeScript 编写的，需要安装 `react` 和 `react-dom` 对应的 TypeScript 接口描述模块 `@types/react` 和 `@types/react-dom` 后才能通过编译。

### 3.4 使用 Vue 框架

Vue 是一个渐进式的 MVVM 框架，想比于 React、Angular 它更灵活。它不会强制性地内置一些功能和语法，你可以根据自己的需要一点一点地添加功能，威力方便编码，大多数项目都会采用 Vue 官方的单文件组件的写法去编写项目。Vue 和 React 一样，它们都推崇组件化和由数据驱动视图的思想，视图和数据绑定再一起，数据改变视图会跟着改变，而无需直接操作视图

#### 3.4.1 接入 Webpack

目前最成熟和流行的开发 Vue 项目的方式都是采用 ES6 加 Babel 转换，这和基本的采用 ES6  开发的项目很相似，区别在于要解析 .vue 格式的单文件组件，好在 Vue 官方提供了对应的 vue-loader 可以很方便的完成单文件组件的转换，修改 Webpack 配置如下：

```js
module: {
  rules: [
    {
      test: /\.vue$/,
      use: ['vue-loader'],
    },
  ]
}
```

vue-loader：解析和转换 .vue 文件，提取出其中的逻辑代码 script，样式代码 style，以及 HTML 模板 template，再分别把他们交给对应的 loader 去处理

#### 3.4.2. 使用 TypeScript 编写 Vue 应用

从 Vue 2.5.0+ 版本开始，提供了对 TypeScript 的良好支持，使用 TypeScript 编写 Vue 是一个很好的选择，因为 TypeScript 能检查出一些潜在的错误

新增 tsconfig.json 配置文件，内容如下：

```json
{
  "compilerOptions": {
    // 构建出 ES5 版本的 JavaScript，与 Vue 的浏览器支持保持一致
    "target": "es5",
    // 开启严格模式，这可以对 `this` 上的数据属性进行更严格的推断
    "strict": true,
    // TypeScript 编译器输出的 JavaScript 采用 es2015 模块化，使 Tree Shaking 生效
    "module": "es2015",
    "moduleResolution": "node"
  }
}
```

Webpack 配置如下：

```js
const path = require('path');

module.exports = {
  resolve: {
    // 增加对 TypeScript 的 .ts 和 .vue 文件的支持
    extensions: ['.ts', '.js', '.vue', '.json'],
  },
  module: {
    rules: [
      // 加载 .ts 文件
      {
        test: /\.ts$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          // 让 tsc 把 vue 文件当成一个 TypeScript 模块去处理，以解决 moudle not found 的问题，tsc 本身不会处理 .vue 结尾的文件
          appendTsSuffixTo: [/\.vue$/],
        }
      },
    ]
  },
};
```

### ,,,

## 4. 优化

优化开发体验的目的是为了提升开发时的效率，其中又可以分为以下几点：

1. 优化构建速度：缩小文件搜索范围、使用 DllPlugin、HappyPack、ParalleUglifyPlugin
2. 优化使用体验：使用自动刷新，开启模块热替换

优化输出质量的目的是为了给用户呈现体验更好的网页，例如减少首屏加载时间、提升性能流畅度等：

1. 减少首屏加载时间：区分环境、压缩代码、CDN 加速、使用 Tree Shaking、提取公共代码、按需加载
2. 提升流畅度，也就是提升代码性能：使用 Prepack、开启 Scope Hoisting

### ，，，

## 5. 原理

了解 Webpack 整体架构、工作流程，学会区分一个功能的实现是通过 Loader 合适还是 Plugin 更合适

### 5.1 工作原理概括

Webpack 以其使用简单著称，在使用它的过程中，使用者只需把它当作一个黑盒，需要关心的只有它暴露出来的配置

#### 5.1.1. 流程概括

Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

1. 初始化参数
2. 开始编译
3. 确定入口
4. 编译模块
5. 完成模块编译
6. 输出资源
7. 输出完成

#### 5.1.2. 流程细节

Webpack 构建流程可以分为以下三大阶段：

1. 初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Comiler
2. 编译：从 Entry 发出，针对每个 Module 穿行调用对应的 Loader 去翻译文件内容，再找到该 Module 依赖的 Module，递归的进行编译处理
3. 输出：对编译后的 Module 组合成 Chunk，把 Chunk 转换成文件，输出文件系统

### ，，，