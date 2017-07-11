## webpack 基础    

### 安装webpack     
 
 作为全局安装 

```
$ npm install webpack -g
$ webpack -v    
```
作为项目依赖安装 

```
$ npm install --save webpack      
```
### webpack简介（配置项有如下几点）

<img src="./images/config.jpeg" width="300" height="400" />

```
* entry: 入口，定义要打包的文件。（即app第一个启动文件）  

* output: 出口，定义打包输出的文件，包括路径（path），文件名（filename），还可能有运行时的访问路径（publicPath）参数。   

* module: webpack将所有的资源都看做是模块，而模块就需要加载器；主要定义一些loaders,定义哪些后缀名的文件应该用哪些loader。

* resolve: 定义能够被打包的文件，文件后缀名 

* plugins: 定义一些额外的插件     

``` 

**入口 (entry) **

```
entry: {
  home: "./home.js",
  about: "./about.js",
  contact: "./contact.js"
}
```

**出口 (output) **

```
output:{
    // path目录对应一个绝对路径。
    path:__dirname+'/public',
    //静态名称：决定了每个输出 bundle 的名称
    filename:'bundle.js'
  },
```

**加载器配置（module）**
 
注：module 的加载顺序是从右往左的    
> 
  * css 文件使用： style-loader，css-loader ，sass-loader或less-loader
  * js 文件使用： babel-loader，babel-preset-es2015，babel-preset-react，jsx-loader
  * 图片文件使用：url-loader、file-loader、image-webpack-loader

**解析 (resolve) **

```
resolve: {
        //查找module的话从这里开始查找
        root: '/Users/dongfanai/react-demo/src', //绝对路径
        //自动扩展文件后缀名，意味着我们require模块可以省略不写后缀名
        extensions: ['', '.js', '.json', '.less'],
        //模块别名定义，方便后续直接引用别名，无须多写长长的地址
        alias: {
        //后续直接 require('kr-ui') 或 import {Title} from 'kr-ui'即可
            'kr-ui' : path.join(process.cwd(), '/src/Components'),
        }
    }
```

**插件 (plugins) **

```
var webpack = require('webpack')
// 导入非 webpack 默认自带插件
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var DashboardPlugin = require('webpack-dashboard/plugin');

// 在配置中添加插件
plugins: [
  // 构建优化插件
  new webpack.optimize.CommonsChunkPlugin({
    name: 'vendor',
    filename: 'vendor-[hash].min.js',
  }),
  new webpack.optimize.UglifyJsPlugin({
    compress: {
      warnings: false,
      drop_console: false,
    }
  }),
  new ExtractTextPlugin({
    filename: 'build.min.css',
    allChunks: true,
  }),
  new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/),
  // 编译时(compile time)插件
  new webpack.DefinePlugin({
    'process.env.NODE_ENV': '"production"',
  }),
  // webpack-dev-server 强化插件
  new DashboardPlugin(),
  new webpack.HotModuleReplacementPlugin(),
]

```













