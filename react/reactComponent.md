# React生命周期

组件的生命周期分可为三大部分： **组件初始化**、**组件更新**、**组件卸载**。

###生命周期

#### 组件初始化   

初始化，第一次render 

* **getDefaultProps**

	只在组件创建时调用一次并缓存返回的对象。   
	* 元件类别被建立时立即调用。       
	* 回传属性若使用物件时则直接参考即所有元件实例公用。   
	* 无法使用 this.state 或 setState()。   

	(es6写法如下)
	```
	static defaultProps = {
			type:'default'
		}
	```
* **getInitialState**

	初始化 this.state 的值，只在组件装载之前调用一次。
	* 必须要回传物件或 null 。   
	* 无法使用 this.state 或 setState()。      

	(es6写法如下)

	```
	constructor(props,context) {
	    super(props,context);
	    this.state = { 
	    	openNewCreate: false
	    };
	  }
	```
* **componentWillMount**

	只会在装载之前调用一次，在 render 之前调用，你可以在这个方法里面调用setState 改变状态，并且不会导致额外调用一次 render。      
	* 使用 setState 会立即更新 state 。   
	* 不会再次触发 render 。   
	* 只会在初始化被调用一次 。   
	```
	componentWillMount(){
		//要修改的内容
	}
	```
* **render**  

	组装生成这个组件的HTML结构（使用原生HTML标签或者子组件），也可以返回 null或者false。   
	* 不能使用setState。   
	* 必要的。  
	* 只能回传一个父元件，其他元件必须在其内部。    
	* 回传 null 或 false 表示不输出。实际上是输出<noscript>标签。   
	* 如果里面有其他子元件，在此事件后才开始子元件的生命周期。   

	```
	render(
		return{
			//父元件，包子元件。
		}
	)
	```
* **componentDidMount**

	只会在装载完成之后调用一次，在 render 之后调用。
	* DOM 加载完成后触发。   
	* 会等到子元件都加载完才触发。

	```
	componentDidMount(){
		//要修改的内容
	}
	```

#### 组件更新
	
组件更新分为两部分 ：props发生改变、state发生改变 

##### props发生改变

* **componentWillReceiveProps(nextProps)**

	* 只要父元件更新，子元件都会收到props。
	* 使用setState不会再次触发render。
	* 没有收到props就略过。

##### props 和 state 公用方法

* **shouldComponentUpdate(nextProps, nextState)**

	* 回传false则后续行为省略。
	* 不可使用setState。
	* 第一次初始化和forceUpdate时不会被触发

* **componentWillUpdate(nextProps, nextState)**

	* 即使是父元件更新重新传入props也会触发。
	* 不可使用setState。

* **render**

    同上

* **componentDidUpdate(prevProps, prevState)**

	* 会等子元件更新完成才触发。


#### 组件卸载

* **componentWillUnmount()**

	* 不可使用setState。


## 总结

###生命周期执行流程

####第一次初始化的流程

	1、displayName 用来设定元件名称
	2、getDefaultProps() 当元件类别被建立时就会触发，且和所有实例物件共享
	3、getInitialState() 开始渲染元件时初始化
	4、componentWillMount() 准备加载 DOM 之前触发
	5、render() 执行输出
		* 如果里面有其他的子元件则依据上面的流程 getInitialState() -> componentWillMount() -> render() -> componentDidMount()
	6、componentDidMount() DOM 完成之后立即触发(会等到子元件都完成才触发)

#### 更新的流程

	1、componentWillReceiveProps(nextProps) 
	   这边使用 setState 不会再触发一次  render。初始化不会被触发
	2、shouldComponentUpdate(nextProps, nextState) 
	   元件是否要更新
	3、componentWillUpdate(nextProps, nextState)
	   将要更新之前，不可以用 setState
	4、render()
	5、componentDidUpdate(prevProps, prevState)

#### 使用setState不会重新render的方法

	* componentWillMount()
	* componentWillReceiveProps()

#### 存取 DOM 的适当时机
	
	* componentDidMount()

#### 不得使用 setState 的事件

	* shouldComponentUpdate(nextProps, nextState)
	* componentWillUpdate(nextProps, nextState)
	* render()



























