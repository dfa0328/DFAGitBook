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

<img src="./images/config.jpeg" width="400" height="500" />

```
* entry: 入口，定义要打包的文件。（即app第一个启动文件）  

* output: 出口，定义打包输出的文件，包括路径（path），文件名（filename），还可能有运行时的访问路径（publicPath）参数。   

* module: webpack将所有的资源都看做是模块，而模块就需要加载器；主要定义一些loaders,定义哪些后缀名的文件应该用哪些loader。

* resolve: 定义能够被打包的文件，文件后缀名 

* plugins: 定义一些额外的插件     

``` 

###  入口起点(Entry Points）

#### 单个入口语法

用法： `entry: string | Array<string>`

**webpack.config.js**

```
const config = {
  entry: {
    main: './src/main.js'
  }
};
module.exports = config;
```

简写

```
const config = {
  entry: './src/main.js'
};

module.exports = config;
```

向 entry 属性传入「文件路径(file path)数组」将创建“多个主入口(multi-main entry)”。传入数组这种方式有助于，在你想要多个依赖文件一起注入，并且将它们的依赖导向(graph)到一个“chunk”时。


#### 对象语法(多个入口语法)

用法： `entry:{[entryChunkName: string]: string|Array<string>}`

**webpack.config.js**

```
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};

module.exports = config;
```
> 对象语法会比较繁琐。然而，这是应用程序中定义入口的最可扩展的方式。            
"可扩展的 webpack 配置"是指，可重用并且可以与其他配置组合使用。这是一种流行的技术，用于将关注点(concern)从环境(environment)、构建目标(build target)、运行时(runtime)中分离。然后使用专门的工具（如 webpack-merge）将它们合并。

### 应用场景

* **分离 应用程序(app) 和 公共库(vendor) 入口**

**webpack.config.js**

```
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
module.exports = config;
```
> 从表面上看，这告诉我们 webpack 从 app.js 和 vendors.js 开始创建依赖图表(dependency graph)。这些图表是彼此完全分离、互相独立的（每个 bundle 中都有一个 webpack 引导(bootstrap)）。这种方式比较常见于，只有一个入口起点（不包括 vendor）的单页应用程序(single page application)中。

>此设置允许你使用 CommonsChunkPlugin 从「应用程序 bundle」中提取 vendor 引用(vendor reference) 到 vendor bundle，并把 vendor 引用的部分替换为 __webpack_require__() 调用。如果应用程序 bundle 中没有 vendor 代码，那么你可以在 webpack 中实现被称为长效缓存的通用模式。


* **多个页面应用程序**

```
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
module.exports = config;

```
> 我们告诉 webpack 需要 3 个独立分离的依赖图表（如上面的示例）。

> 在多页应用中，服务器将为你获取一个新的 HTML 文档。页面重新加载新文档，并且资源被重新下载。然而，这给了我们特殊的机会去做很多事：使用 CommonsChunkPlugin 为每个页面间的应用程序共享代码创建 bundle。由于入口起点增多，多页应用能够在入口起点重用大量代码/模块，这样可以极大的从这些这些技术受益。


###  输出(Output）

此选项影响 compilation 对象的输出。`output` 选项控制 webpack 如何向硬盘写入编译文件。注意，即使可以存在多个`入口`起点，但只指定一个`输出`配置。

如果你用了哈希（`[hash]` 或 `[chunkhash]`），请确保模块具有一致的顺序。可以使用 `OccurrenceOrderPlugin` 或 `recordsPath`。

#### 基本语法

用法：在 webpack 中配置 `output `属性的最低要求是，将它的值设置为一个对象，包括以下两点：
编译文件的`文件名(filename)`，首选推荐：`// main.js || bundle.js || index.js`

`output.path` 对应一个绝对路径，此路径是你希望一次性打包的目录。

**webpack.config.js**

```
const config = {
  output: {
    filename: 'bundle.js',
    path: '/home/proj/public/assets'
  }
};

module.exports = config;
```



















