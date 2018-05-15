> webpack 是一个现代 JavaScript 应用程序的静态模块打包器。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

# 入口
- 入口(entry)起点指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。默认值为 ./src。
```
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```
- ### 语法
1. 单个入口 
```
entry: string|Array<string>
```
向 entry 属性传入「文件路径(file path)数组」将创建“多个主入口“，适用想要多个依赖文件一起注入，并且将它们的依赖导向到一个“chunk”时
2. 对象
```
entry: {[entryChunkName: string]: string|Array<string>}
```
应用程序中定义入口的最可扩展的方式

- ### 实际用例
1. 分离应用程序(app) 和 第三方库(vendor) 入口
```
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```
依赖图是彼此完全分离、互相独立的（每个 bundle 中都有一个 webpack 引导(bootstrap)）。
此设置允许使用 CommonsChunkPlugin 从「应用程序 bundle」中提取 vendor 引用到 vendor bundle，并把引用 vendor 的部分替换为 __webpack_require__() 调用。如果应用程序 bundle 中没有 vendor 代码，那么你可以在 webpack 中实现被称为长效缓存的通用模式。

2. 多页面应用程序

```
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
```
在多页应用中，每当页面跳转时服务器将为你获取一个新的 HTML 文档。页面重新加载新文档，并且资源被重新下载。然而使用 CommonsChunkPlugin 为每个页面间的应用程序共享代码创建 bundle。由于入口起点增多，多页应用能够复用入口起点之间的大量代码/模块，从而可以极大地从这些技术中受益。
- - -
# 出口
- 出口(output)属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。

```
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```
# loader
- loader 让 webpack 能够去处理那些非 JavaScript 文件。loader 能够 import 导入任何类型的模块（例如 .css 文件），这是 webpack 特有的功能，其他打包程序或任务执行器的可能并不支持。
1. test 属性，用于标识出应该被对应的 loader 进行转换的某个或某些文件
2. use 属性，表示进行转换时，应该使用哪个 loader。

```
const path = require('path');

const config = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};

module.exports = config;
```
在 require()/import 语句中被解析为 '.txt' 的路径时，在对它打包之前，先使用 raw-loader 转换一下
- 在 webpack 配置中定义 loader 时，要定义在 module.rules 中，而不是 rules

# 插件
- 插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。
- 要使用一个插件，只需要 require() 它，然后把它添加到 plugins 数组中。可以通过使用 new 操作符来创建它的一个实例，在一个配置文件中因为不同目的而多次使用同一个插件。
```
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

const config = {
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

# 模式
- 通过选择 development 或 production 之中的一个，来设置 mode 参数，可以启用相应模式下的 webpack 内置的优化。
```
module.exports = {
  mode: 'production'
};
```
