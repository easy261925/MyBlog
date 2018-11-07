---
title: 实战项目总结一 (to8to项目基础配置)
date: 2018-03-18 11:58:31
tags: 技术总结 webpack to8to react
categories: React webpack
---

# 实战项目总结一 (to8to项目基础配置)


## `webpack` 的配置 (@3.11.0)


#### `webpack.base.js` 文件

#### loader
> `eslint` 代码格式规范: `eslint-loader` 
>  
> `js` 处理 js 文件，可以使用 ES6 等高级语法: `babel-loader`
> 
> 开发和生产环境都使用了 `less`: `style-loader,css-loader,less-loader`
> 
>  图片处理: `url-loader`
> 
> 
#### plugins

>`HTMLWebpackPlugin` 打包后的 `js` 文件放入到指定 `.html` 文件中 
>

#### 项目路径别名 (alias)

```js
 Common: path.join(__dirname, '..', 'src/components/common/')
```

#### 代码
`webpack.base.js`

```js
const path = require('path')
const HTMLWebpackPlugin = require('html-webpack-plugin')

// 合并路径方法
const resolve = (dir) => {
  return path.join(__dirname, '..', dir)
}

module.exports = {
  entry: {
    bundle: path.join(__dirname, '../src/index.js')
  },
  output: {
    path: path.resolve('dist'),
    filename: '[name].[hash:7].js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: [
          /node_modules/, /build/, resolve('src/router/')
        ], // 不需要规范代码的文件夹
        loader: 'eslint-loader',
        enforce: 'pre', // 编译之前
        include: [
          resolve('src'), resolve('test') // 需要规范代码的文件夹
        ],
        options: {
          formatter: require('eslint-friendly-formatter')
        }
      }, {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              '@babel/preset-env', '@babel/preset-react'
            ],
            'plugins': [
              [
                'import', {
                  'libraryName': 'antd',
                  'libraryDirectory': 'es',
                  'style': 'true'
                }
                // 按需加载 antd
              ],
              ['@babel/plugin-transform-runtime', {
                'helpers': false,
                'polyfill': false,
                'regenerator': true,
                'moduleName': '@babel/runtime'
              }
              	// 项目支持 async/await等高级语法
              ],
              ['transform-react-remove-prop-types']
              // 生产环境删除 prop=types
            ]
          }
        },
        exclude: /node_modules/ // 不需要打包的文件夹
      }, {
        test: /\.less$/,
        use: [
          {
            loader: 'style-loader'
          }, {
            loader: 'css-loader'
          }, {
            loader: 'less-loader'
          }
        ]
      }, {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
              name: 'assets/images/[hash:8].[name].[ext]'
            }
          }
        ]
      }
    ]
  },
  plugins: [new HTMLWebpackPlugin({template: 'index.html', filename: 'index.html', inject: 'body'})],
  resolve: {
    alias: {
      Common: path.join(__dirname, '..', 'src/components/common/'),
      Pages: path.join(__dirname, '..', 'src/pages/'),
      Styles: path.join(__dirname, '..', 'src/assets/style'),
      Component: path.join(__dirname, '..', 'src/components/'),
      CommonActions: path.join(__dirname, '..', 'src/actions/commonActions'),
      CommonReducers: path.join(__dirname, '..', 'src/reducers/commonReducers'),
      Images: path.join(__dirname, '..', 'src/assets/image'),
      Datas: path.join(__dirname, '..', 'src/assets/data')
    }
  }
}
```
 

--
#### `webpack.dev.js` 文件

使用 `webpack-merge` 合并 `webpack.base.js`

#### loader

处理 css 文件 (因为已经采用了 `less` 作为 `css` 样式处理), 此 `loader` 可以省略

#### devtool

>`devtool: "inline-source-map"`
>
>生成一个 DataUrl 形式的 SourceMap 文件.

#### devServer

```
devServer: {
    hot: true,  // 热处理
    inline: true,
    proxy: { // 跨域代理
      '/api': {
        target: 'https://example.com/',
        changeOrigin: true,
        pathRewrite: {'^/api': ''}
      }
    }
  }
```

#### plugins

`NamedModulesPlugin` `HotModuleReplacementPlugin` 协助处理文件热更新

#### 代码

```js
const merge = require("webpack-merge");
const base = require("./webpack.base");
const webpack = require("webpack");

module.exports = merge(base, {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          "style-loader", "css-loader"
        ],
        exclude: /node_modules/
      }
    ]
  },
  devtool: "inline-source-map",
  devServer: {
    hot: true,
    inline: true,
    proxy: {
      '/api': {
        target: 'https://exmaple.com/',
        changeOrigin: true,
        pathRewrite: {'^/api': ''}
      }
    }
  },
  plugins: [
    new webpack.NamedModulesPlugin(),
    new webpack.HotModuleReplacementPlugin()
  ]
})
```

--
#### `webpack.prod.js` 文件

#### devtool

>`source-map` 
>
>解决开发代码与实际运行代码不一致时，帮助我们 debug 到原始开发代码

#### loader

> 把 css 样式为 .html 中的 style
> 
> `extract-text-webpack-plugin`
> 

#### plugins
`extract-text-webpack-plugin` 处理 `css` 样式为 `style`

`clean-webpack-plugin` 删除 `dist` 文件夹

`webpack.optimize.CommonsChunkPlugin` 提取项目中的公共代码部分到 `common.js`中

`webpack.DllReferencePlugin` 将第三方插件/库与项目中的代码分开打包，提升打包效率

`uglifyjs-webpack-plugin` 丑化/压缩打包后的代码

#### 代码

```js
const merge = require("webpack-merge");
const base = require("./webpack.base");
const ExtractTextWebpackPlugin = require("extract-text-webpack-plugin");
const CleanWebpackPlugin = require("clean-webpack-plugin");
const UglifyjsWebpackPlugin = require("uglifyjs-webpack-plugin");
const webpack = require("webpack");
const path = require("path");

module.exports = merge(base, {
  // entry: {
  //  vendor: ["react","react-dom"]
  // },
  dependencies: ["./lib/vendor.js"],
  devtool: "source-map",
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextWebpackPlugin.extract({fallback: "style-loader", use: "css-loader"}),
        exclude: /node_moudles/
      }
    ]
  },
  plugins: [
    new ExtractTextWebpackPlugin("styles.css"),
    new CleanWebpackPlugin(["dist"]),
    // new UglifyjsWebpackPlugin({sourceMap: false}),
    new webpack
      .optimize
      .CommonsChunkPlugin({name: "common", filename: "common.js"}),
    // new webpack
    //   .optimize
    //   .CommonsChunkPlugin({
    //     name: ["vendor","common"],
    //     // filename: "vendor.js" (给 chunk 一个不同的名字)
    //     minChunks: Infinity,
    //     // (随着 entry chunk 越来越多， 这个配置保证没其它的模块会打包进 vendor chunk)
    //   })
    new webpack.DllReferencePlugin({
      manifest: path.resolve(__dirname, "../lib/manifest.json")
    })
  ]
});

```

--
#### `webpack.dll.js` 文件

#### entry

需要抽离出来的第三方库

`react` `react-dom` `react-router-dom` `redux` `react-redux`

#### output

抽离出来的文件出口配置,放到 `/lib/vendor.js`

#### plugins

`webpack.DllPlugin` 将第三方库抽离出来并打包

`uglifyjs-webpack-plugin` 丑化/压缩代码

#### 代码
```js
const webpack = require("webpack");
const path = require("path");
const UglifyjsWebpackPlugin = require("uglifyjs-webpack-plugin");

module.exports = {
  entry: [
    "react",
    "react-dom",
    "react-router-dom",
    "redux",
    "react-redux"
    /** ... 等其他依赖库 */
  ],
  output: {
    path: path.resolve("lib"),
    filename: "vendor.js",
    library: "vendor_[hash]"
  },
  plugins: [
    new webpack.DllPlugin({
      name: "vendor_[hash]",
      path: path.resolve("lib/manifest.json")
    }),
    new UglifyjsWebpackPlugin()
  ]
}
```

