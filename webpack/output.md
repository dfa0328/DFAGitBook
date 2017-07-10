##  输出(Output）

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
