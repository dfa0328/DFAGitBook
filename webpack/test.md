## webpack + react-router 按需加载

>重点在于react-router的getComponent方法和webpack的require.ensure方法的搭配使用.

#### 需要react按需加载的场景

使用react做项目的过程中大家可能会发现一个问题,就是首次打开使用react制作的网站时它的首次渲染速度都比较慢,特别是一些相对来说比较大型的网站.这是因为react+webpack的开发模式下默认会把你整个项目打包成一个js文件,当你在首次渲染时,它必须下载你整个项目的代码,然后才开始渲染页面,如果你的项目代码量非常多的话这样就会导致你的整个项目首次渲染速度非常的慢,此时按需(模块化)加载也就派上用场了.


### 怎么设置按需加载

#### webpack设置部分

其实就是在webpack.config.js配置文件中,开启分块打包模式(chunkFilename),在文件出口处添加chunkFilename: '[name].[chunkhash:5].chunk.js'这一项.其中[name]是指文件名(未赋值时默认是文件的id),[chunkhash:5]指提取hash值的前五位数字,要用hash值前五位数来命名的好处是在开发模式中未修改代码的页面不会重新打包,提升开发速度

```
 output: {
    filename: 'application.js',
    path: path.join(__dirname, './dist'),
    publicPath: "/dist/",
    chunkFilename: '[name].[chunkhash:5].chunk.js'
  },
```

#### 路由设置部分

在webpack中为我们提供了require.ensure这么一个加载函数,其中它接受三个参数,第一个参数是依赖插件(较少用到),第二个参数是一个回调函数(重点,这个回调函数是我们的打包逻辑),第三个参数是分包块文件的文件名(对应webpack.config.js中 chunkFilename中[name]这个字段).利用require.ensure函数搭配react-router中的getComponent组件加载函数来使用,就能成功设置按需加载了:

1、原来的路由文件写法:

```
import React from 'react'
import { Route , IndexRoute } from 'react-router'
import App from './containers/App'

import Detail from './components/Detail'

import NotFound from './components/NotFound'
import Home from './components/Home'

export default (
  <Route>
  	<Route path="/" component={App}>
  		<IndexRoute component={Home} />
  		<Route path="/home" component={Home} />
  	</Route>
  </Route>
)
```
2、修改为按需加载的路由文件写法:

```
import React from 'react'
import { Route , IndexRoute } from 'react-router'
//公共组件按正常方式渲染
import App from './containers/App'

//需要按需加载的页面getComponent方法
const Home = (location, callback) => {
  require.ensure([], require => {
    callback(null, require('./components/Home').default)
  }, 'home')
};

//使用getComponent方法渲染按需加载的页面
export default (
	<Route>
		<Route path="/" component={App}>
			<IndexRoute getComponent={Home} />
			<Route path="/home" getComponent={Home} />
		</Route>
	</Route>
)
```


**注意点**：

 1、 require('components/Index').default中require方法的参数不能使用变量，只能使用字符串！

 2、 如果你的组件是使用es5的module.exports导出的话，那么只需要require('components/Index')即可。而如果你的组件是使用es6的export default导出的话，那么需要加上default！例如：require('components/Index').default

 3、 如果在路由页面使用了按需加载（require.ensure）加载路由级组件的方式，那么在其他地方（包括本页面）就不要再import了，否则不会打包生成chunk文件。简而言之，需要按需加载的路由级组件必须在路由页面进行加载。













