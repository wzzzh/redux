# redux #
	redux是一个可预测的状态管理的库

### 一.redux的使用环境（场景） ###
	 多交互、多数据源。  
### 例-使用环境 ###

	1.用户的使用方式复杂  
	2.不同身份的用户有不同的使用方式（比如普通用户和管理员）  
	3.多个用户之间可以协作   
	4.与服务器大量交互，或者使用了WebSocket  
	5.View要从多个来源获取数据  
### 例-组件的使用 ###
	1.某个组件的状态，需要共享  
	2.某个状态需要在任何地方都可以拿到  
	3.一个组件需要改变全局状态  
	4.一个组件需要改变另一个组件的状态
### 二.redux的设计思想： ###
	（1）Web 应用是一个状态机，视图与状态是一一对应的。
	（2）所有的状态，保存在一个对象里面。
###三、 API ###
3.1.  
**store:**一个存数据的容器。**整个应用**只能有**一个store**  
注：redux 提供了一个函数用于生成stroe->createStore  

	import {createStore} from 'redux'
	const store = createStore(fn);
上面代码中：createStore接收一个函数作为参数，返回一个新的store对象

3.2  
state:某个时点（想要得到的某堆数据）数据的集合  
解：store对象包含所有数据，如果想要某个时点的数据就要通过**stroe.getState()**拿到
	
	import {createStore} from 'redux'
	const store = createStore(fn);
	const state = store.getState();		//此时就拿到了store里的state数据
redux规定，一个state对应一个view(视图)	。只要state相同，那么视图就是相同的。  
更直白的说法：state是见不得光的是属于超级管理员的，只能通过视图的方式展示给用户，且用户没有权限直接改变	  

3.3  
Action:改变状态（state）时发出的信号->如果有状态改变的需求， 需要有相应的action  
&emsp;由于state用户不可见，用户只能通过改变视图来改变state,而此时action就是视图发起的通知，表示state要发生改变  
**注：**action是一个对象，其中的type值是必须的，表示要发起action的名称。同时其他属性也可以自定义。

	 const action = {type:"INCREMENT",val}
	//此时，INCREMENT即是action的名称，val即为传的值，其他属性自定义

3.4：  
Action创建函数：只要用户想要改变信息。视图就要发起action创建函数(action creator)  
	
	const todo = 'addTodo';
	fucntion addtodo(val){type:"ADDTODO",val};
	const action = (addtodo('学习redux'))

3.5  
**store.dispatch():**是view发出action的唯一方法  
注：dispatch要接受的**一定**是一个action || action发起的创建函数
	
	1.接收action对象：
		store.dispatch({type:"ADDTODO",'学习redux'})
	2.接收action的创建函数
		store.dispatch（addtodo('学习redux')）

3.6  
**reducer:**  
Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。

Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。
	
	const reduce = function(state,action){
		...
		return newState;
	}
实际应用中：  
&emsp;&emsp;Reducer 函数不用手动调用，store.dispatch方法会触发 Reducer 的自动执行。为此，Store 需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入createStore方法。

	import {createStore} from 'redux';
	fucntion reducer(state,action){
		...
		return newState;
	}
	
	const store = new createStore(reducer);

上面代码中，createStore接受 reducer 作为参数，生成一个新的 Store。以后每当store.dispatch发送过来一个新的 Action，就会自动调用 reducer，得到新的 State。
	
3.7
**store.subscribe():**  
Store 允许使用store.subscribe方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。  
  

###四、store的实现 ###	：
4.1：  
store的三个方法：

	store.getState()：获取到当前时点的state
	store.dispatch():发起action
	store.subscribe():监听函数，用于view渲染

4.2：  
**同步操作：**action发出后，reducer会立即算出state的值并返回。即为同步操作  
**异步操作：**action发出后，reducer过一段时间再执行reducer。即为异步操作  
	此时，要做到异步操作就要通过中间件（applyMiddleware）

	import {createStore,applyMiddleware} from 'redux';
	import thunk from 'redux-thunk';
	function reducer(state,action){
		....
		return newState;
	}
	const store = createStore(reducer,applyMiddleware(thunk));

中间件会将每次的dispatch拦截下来，判断其发起的action是什么类型，如果是对象，那么直接会通过reducer算出state的值并返回，如果返回的是函数，那么会继续执行这个函数，如果函数有东西，那么会继续执行，如果没有东西，那么不做任何操作，知道dispatch返回的是一个对象为止

### redux的具体步骤： ###
1.引入：  
&emsp;&emsp;import {createStore,applyMiddleware} from 'redux';  
&emsp;&emsp;import thunk from 'redux-thunk'  
2.创建reducer  
&emsp;&emsp;function reducer(state,action){    
&emsp;&emsp;&emsp;&emsp;....  
&emsp;&emsp;&emsp;&emsp;return newState   
&emsp;&emsp;&emsp;&emsp;}  
3.挂载store实例：  
&emsp;&emsp;const store =createStore(reducer,applyMiddleware(thunk));  
4.监听函数，渲染视图：  
&emsp;&emsp;store.subscribe(){}	
	




