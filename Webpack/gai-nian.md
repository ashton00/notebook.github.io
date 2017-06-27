

## Webpack的四大概念

```
Entry
Output
Loaders
Plugins
```

## Entry

webpack 打包模块是通过一个模块依赖图

entry 作为 webpack 的入口，告诉 webpack 从哪里开始加载文件


## Output


```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

filename 指定处理哪些文件，path 指定输出的路径是什么

## Loaders

Webpack 虽然将每一个文件都视为模块，但实际上它只认识 Js 文件，所以在添加到依赖图之前，非JS文件都会被先转化为模块，再加载到依赖图

所以我们需要用到 Loaders 来加载不同的文件


```js
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      /* 通过 raw-loader 来处理 .txt 文件*/
      {test: /\.txt$/, use: 'raw-loader'}
    ]
  }
};

module.exports = config;
```

> 注意这里是 module.rules，不是 rule


## Plugins

Loaders 是在单个文件级别进行操作，

更多的时候，我们需要对读进来的文件“块”进行一些操作，或者对文件进行“编译”，这时候 Plugins 就可以发挥比较大的作用

比如我们需要用 babel 对 es6 文件进行转义等等，我们都可以通过 Plugins 来做

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      {test: /\.txt$/, use: 'raw-loader'}
    ]
  },
  /* 引入处理的插件 */
  plugins: [
    // 做混淆
    new webpack.optimize.UglifyJsPlugin(),
    // 简化webpack的html中，方便引入 webpack bundles
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;

```

当然，引用 Plugins 的时候，也需要对文件进行 require


Plugins 的列表 : [Plugins](https://webpack.js.org/plugins/)
