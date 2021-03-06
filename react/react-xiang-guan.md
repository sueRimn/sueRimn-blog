# React相关



#### 1.组件内部状态`state`的修改

* 修改组件的每个状态，组件的`render()`方法都会再次运行。这样就可以修改组件内部状态，确保组件重新改渲染并且展示从内部状态获取到的正确数据
* 初始状态应该通过`this`绑定到类上

```text
  class App extends Component {
    //构造函数在一个组件周期只执行一次
    constructor(props) {
      super(props);
      //state通过this绑定在类上  可以在整个组件中访问到state
      this.state = {
        ...
      };
    }
    render(){
    ...
    }
  }
```

* 注意：不能直接修改`state`，必须使用`setState()`方法进行修改

#### 2.对象初始化

* 简写属性名

```text
  const name = 'sueRimn';
    const user = {
    //属性名与变量名相同时
      name,
    };
  }
```

* 简写方法名

```text
  const userService = {
    const key = 'name';
    const user2 = {
      [key] :'sueRimn',
  };
```

#### 3.单项数据流

* 在类内部绑定方法

```text
  class App extends Component {
    //构造函数只应该实例化类以及所有属性，不应该处理业务逻辑
    constructor(props) {
      super(props);
      //state通过this绑定在类上  可以在整个组件中访问到state
      this.state = {
        ...
      };
      //在构造函数中绑定它，定义为类方法 this指向类实例
      this.onDismiss = this.onDismiss.bind(this);
    }
    render()
   }
```

* 使用`ES6`箭头函数，自动绑定到类

```text
  class App extends Component {
    constructor(props) {
      super(props);
      this.state = {
        ...
      };
    }
    //在类外定义它的功能和业务逻辑 
    onDismiss = id => {//箭头函数
      ...
    }
    render(){
    }
  }
```

#### 4.事件处理

* 在外部定一个包装函数，并且只将定义的函数传递给处理程序
* 使用箭头函数

#### 5.与表单交互

* 需要将输入的只存在本地状态中，使用合成事件访问事件返回值
* 使用高阶函数（在组件外定义）：一个函数返回另一个函数

#### 6.`ES6`解构

> 访问对象和数组的属性，叫解构

#### 7.受控组件

* 将不受控组件改为受控

#### 8.拆分组件

* `props`通过`this`访问

#### 9.可组合组件

* `props`中有一个属性`children`可供使用，通过它可以将元素从上层传递到组件中
* 不仅可以传递文本为子元素给组件，还可以将一个元素或者元素数（组件）作为子元素传递

#### 10.可复用组件

* 函数式无状态组件：
  * 就是函数
  * 无本地状态，不能使用`setState()`和`this.state`更新或者访问状态
  * 接收一个输入并返回一个输出，输入的是`props`，输出的是`JSX`组件实例。
  * 没有生命周期方法
* ES6类组件：
  * 在类的定义中，它们继承自`React`组件
  * `extend`会注册所有的生命周期方法，只要在`React component API`中，都可以在组件中使用
  * 可以使用`this.state`和`setState()`
* `React.createClass`：不推荐使用

 写法区别如下：

```text
 //定义组件 props通过this可以访问
 //1.ES6组件类写法
 class Search extends Component{
   render(){
     const {value, onChange, children} = this.props;
     return(
       <form>
         {children}
         <input
           type="text"
           value={value}
           onChange={onChange}
         >
         </input>
       </form>
     )
   }
 }
//2.函数无状态类组件写法
const Search = ({value, onChange, children}) =>
<form>
  {children}
  <input
    type="text"
    value={value}
    onChange={onChange}
  >
  </input>
</form>
```

当组件不需要使用更改内部状态且无生命周期的时候定义组件为函数式无状态组件； 当需要访问`state`或者生命周期方法时，就用`ES6`类组件

#### 11.给组件声明样式

#### 12.使用真实`API`

* 生命周期方法（执行顺序排序）：
  * `constructor()`构造函数：只有在组件实例化（挂载）并插入到`DOM`中的时候才会被调用
  * `componentWillMount()`：组件挂载中
  * `render()`：在组件挂载的时候调用，同时当组件更新的时候也会被调用。每当组件的状态或者属性改变时，就会被调用
  * `componentDidMount()`：组件挂载结束
* 当组件的状态或者属性发生改变时生命周期的变化（5个方法用于组件更新）：
  * `componentWillReceiveProps()`：
  * `shouldComponentUpdate()`
  * `componentWillUpade()`
  * `render()`
  * `componentDidUpdate()`
* 组件卸载也有生命周期：只要一个-`componentWillUnmount()`
* 生命周期方法的适用场景：
  * `constructor(props)`:在组件初始化时被调用，可以**设置初始化状态以及绑定类方法**
  * `componentWillMount()`:在`render()`之前被调用，可以**设置组件内部状态**，因为不会触发组件的再次渲染
  * `render()`:**返回作为组件输出的元素**。应该作一个纯函数，不应该在此修改组件的状态。它把属性和状态作为输入并且返回（需要渲染的）元素
  * `componentDidMount()`：仅在组件挂载后执行一次。是**发起异步请求去API获取数据**的最佳时期；获取到的数据将被保存在内部组件的状态中然后 在`render()`生命周期方法中展示出来
  * `componentWillReceiveProps(nextProps)`: 在一个更新生命周中被调用，新的属性会作为它的输入。因此可以利用 `this.props` 来对比之后的属性和 之前的属性，基于对比的结果去实现不同的行为。此外，可以基于新的属性来设置组件的状态
  * ·shouldComponentUpdate\(nextProps, nextState\)·: 每次组件因为状态或者属性更改而更新时，它都会被调用，**使用它来进行性能优化**
  * `componentWillUpdate(nextProps, nextState)`： 这个方法是 `render()` 执行之前的最后一个方法。 利用这个方法在渲染之前进行最后的准备。 注意在这个生命周期方法中不能再触发 `setState()`。如果你想基于新的属性计算状态，必须利用`componentWillReceiveProps()`
  * `componentDidUpdate(prevProps, prevState)` :这个方法在 `render()` 之后立即调用。可以用它当成操作 `DOM`或者执行更多**异步请求**的机会。
  * `componentWillUnmount()`: 它会在组件销毁之前被调用。 可以利用这个生命周期方法去**执行任何清理任务**
  * `componentDidCatch(error, info)`： 用来**捕获组件的错误**
* 获取数据
* 扩展运算符
* 条件渲染
  * 使用jsx的if-else
  * 三元运算符
  * &&逻辑运算符
* 客户端或服务端搜素

#### 13.高级React组件

* 引用DOM元素
* 加载
* 高阶组件（HOC）
  * 使用with前缀命名

