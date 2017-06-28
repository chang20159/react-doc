>[React.Component](https://facebook.github.io/react/docs/react-component.html)

# React.Component
使用组件可以将UI拆分成独立的可重复使用的部分，并可单独考虑每个部分。 React.Component由React提供。

## 总览
React.Component是一个抽象基类，直接引用React.Component没什么意义。 我们通常会将其子类化，并至少需要定义一个render()方法。

通常我们将一个React组件定义为JavaScript类：

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
如果没有使用ES6，则可以使用create-react-class模块。

### 组件生命周期
每个组件都有几个“生命周期方法”，我们可以在此过程中的特定时间覆盖运行代码。 用will作为方法前缀表示会在事情发生之前被调用，用did作为方法前缀表示在事情发生之后调用。

#### Mounting
当创建组件实例并将其插入到DOM中时，将调用这些方法：

- ```constructor()```
- ```componentWillMount()```
- ```render()```
- ```componentDidMount()```

#### Updating
当 props 或 state改变时会触发重新渲染，这些方法在重新渲染时会被调用：

- ```componentWillReceiveProps()```
- ```shouldComponentUpdate()```
- ```componentWillUpdate()```
- ```render()```
- ```componentDidUpdate()```

#### Unmounting
当组件从DOM中移除时会调用这个方法：

- ```componentWillUnmount()```

### 其他API
组件还提供了其他一些API：

- ```setState()```
- ```forceUpdate()```

### 类属性

- ```defaultProps```
- ```displayName```

### 实例属性

- ```props```
- ```state```

## 参考

### render()
```javascript
render()
```
render()方法是必须的。调用该方法时，会检查this.props和this.state并返回一个React元素。 此元素可以是原生DOM例如&lt;div />，也可以是自己定义的一个复合组件。

也可以返回null或false表示不呈现。 返回null或false时，ReactDOM.findDOMNode(this)将返回null。

render() 方法必须是纯的，不能修改组件的state。对于相同的props和state每次调用都返回相同的结果，并且它不会直接与浏览器交互。

如果需要与浏览器交互，请在componentDidMount()或其他生命周期方法中执行操作。 

**注意，如果shouldComponentUpdate()返回false，则不会调用render()**

### constructor()
```javascript
constructor(props)
```

在装载组件之前调用构造函数。 在实现React.Component子类的构造函数时，您应该在任何其他语句之前调用super(props)。 否则，在构造函数中this.props是未定义的，这可能会导致错误。

初始化state需要在构造函数中执行。 如果不初始化state也不绑定方法，就不需要实现构造函数。

也可以用props初始化state，这是用最初的props来设定state的一种方法。 示例：

```javascript
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
```
注意这种方式下，state不会随着任何prop的更新而更新。 我们通常并不想将props同步到state，而是选择提升状态。

如果需要将props同步到state，可能还需要实现componentWillReceiveProps(nextProps)来保持state与其最新prop同步。 但是还是提升state容易一些，而且不容易出错。

### componentWillMount()
```javascript
componentWillMount()
```
componentWillMount()在装载之前调用。**因为在render()之前调用，所以在此方法中设置state不会触发重新渲染。**

这是在服务端渲染唯一会调用的生命周期方法。 一般来说，我们建议使用constructor(),而不要使用此方法。

### componentDidMount()
```javascript
componentDidMount()
```
在安装组件后立即调用componentDidMount()。 DOM节点的初始化操作应该放在这里， 如果需要从远程服务器加载数据，这也是发起网络请求的好地方。另外，**在此方法中设置state将触发重新渲染。**

### componentWillReceiveProps()
```javascript
componentWillReceiveProps(nextProps)
```
在已安装组件接收新的props之前调用componentWillReceiveProps()，如果需要更新state来响应props的变化，可以比较this.props 与 nextProps ，再调用this.setState()。

**注意，即使props没有改变，React也可以调用componentWillReceiveProps**，因此如果只想在props有变化时操作，请确保比较当前props和下一个props。 

在安装过程中，React不会使用初始props调用componentWillReceiveProps。 只有当组件的props可能更新时才会调用此方法。 

**调用this.setState通常不会触发componentWillReceiveProps。**

### shouldComponentUpdate()
```javascript
shouldComponentUpdate(nextProps, nextState)
```
在shouldComponentUpdate()中根据props和state的更改决定是否渲染组件。默认是在每个state更改时重新渲染，而在绝大多数情况下，您应该依赖于默认行为。

在接收新的props或state时，在render之前调用shouldComponentUpdate()，默认情况下返回true,表示需要执行render()方法。

**在初次渲染或则使用forceUpdate()时不执行此方法。**

**返回false时不会阻止子组件在state更改时重新渲染。**

目前，如果shouldComponentUpdate()返回false，那么将不会调用componentWillUpdate()，render()和componentDidUpdate()。

**请注意，在将来，React可能将shouldComponentUpdate()作为一个提示而不是一个必须执行的指令来处理，返回false时仍然可能导致组件重新渲染。**

如果在分析后确定某个组件较慢，则可以将其更改为继承自实现了shouldComponentUpdate()的React.PureComponent，它对props和state进行了浅比较。如果您有信心，要手工编写，可以将this.props与nextProps和this.state与nextState进行比较，并返回false，告知React可以跳过更新。

### componentWillUpdate()
```javascript
componentWillUpdate(nextProps, nextState)
```
当接收到新的props或state时，将在render之前立即调用componentWillUpdate()。 初始化渲染不会调用此方法。

**请注意，不能在此处调用this.setState()。** 如果需要更新state以响应props，请改用componentWillReceiveProps()。

注意，如果shouldComponentUpdate()返回false，那么将不会调用componentWillUpdate()

### componentDidUpdate()
```javascript
componentDidUpdate(prevProps, prevState)
```
更新后立即调用componentDidUpdate(), 初始化渲染不会调用此方法。

当组件更新时，可以在componentDidUpdate()中进行DOM操作。 只要您将当前props与以前的props进行比较，也可以在这里发起网络请求（如果props没有改变，则网络请求可能不是必需的）。

**注意，如果shouldComponentUpdate()返回false，那么将不会调用componentDidUpdate()。**

### componentWillUnmount()
```javascript
componentWillUnmount()
```
在组件被卸载并销毁之前，会立即调用componentWillUnmount()。 可以在此方法中执行任何必要的清理，例如清除定时器，取消网络请求或清除在componentDidMount中创建的任何DOM元素。

### setState()
```javascript
setState(updater, [callback])
```
setState()将对组件状态的更改进行排队，并告诉React，该组件及其子组件需要更新状态重新渲染。 这是用于更新用户界面以响应事件处理程序和服务器响应的主要方法。

可以将setState()认为是更新组件的一个请求而不是立即执行的命令。 为了更好的渲染性能，React可能会延迟执行，然后一次更新多个组件。 React不能保证state的更改会立即提现在UI上。

setState()并不总是立即更新组件,它可能会批量或延迟更新。 如果在调用setState()之后立即读取this.state会出现问题。我们可以使用componentDidUpdate或setState回调（setState(updater，callback)），他们会在更新完成之后触发执行。 如果需要根据先前的state设置state，请看下面updater参数的介绍。

调用setState()一定会触发重新渲染，除非shouldComponentUpdate()返回false。 如果使用的是可变对象并且无法在shouldComponentUpdate()中实现条件呈现逻辑，则仅当新state与先前state不同时才调用setState()将避免不必要的重新渲染。

**第一个参数是一个updater函数：**

```javascript
(prevState, props) => stateChange
```

prevState是对以前状态的引用,不应该直接改变。 相反，应该根据prevState和props的输入构建一个新对象来表示更改。 例如，假设我们想通过props.step来增加state中的值：


```javascript
this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});
```
updater函数接收到的prevState和props都保证是最新的。 updater的输出与prevState进行了浅层合并。

setState()的第二个参数是一个可选的回调函数，当setState完成并且该组件被重新渲染后，该函数将被执行。 通常我们建议使用componentDidUpdate()完成这样的逻辑。

您可以选择将一个对象作为第一个参数传递给setState()而不是一个函数:

```javascript
setState(stateChange, [callback])
```

这将会把stateChange的浅合并到新的state，例如调整购物车商品数量：

```javascript
this.setState({quantity: 2})
```
这种形式的setState()也是异步的，同一声明周期中的多次调用可以放在一起批处理。 例如，如果您尝试在同一周期内多次增加项目数量，结果相当于：


```javascript
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1},
  ...
)
```
后续调用将覆盖同一周期中先前调用的值，因此数量只会增加一次。 如果下一个状态取决于之前的状态，我们建议使用updater函数形式：

```javascript
this.setState((prevState) => {
  return {counter: prevState.quantity + 1};
});
```
更多信息可以参考 [State和生命周期](../React快速开始/React快速开始（五）State和生命周期.md)

### forceUpdate()
```
component.forceUpdate(callback)
```
默认情况下，当您的组件的state或props更改时，组件将重新渲染。 如果您的render()方法依赖于某些其他数据，您可以通过调用forceUpdate()来告诉React该组件需要重新渲染。

**调用forceUpdate()会导致在组件上调用render()，并跳过shouldComponentUpdate()方法。** 这将触发子组件的正常生命周期方法，包括每个子组件的shouldComponentUpdate()方法。 如果标记更改，则React仍将仅更新DOM(这句没懂。。。)。

通常你应该尽量避免使用forceUpdate()，render()的触发执行应该只依赖this.props和this.state。

## 类属性

### defaultProps
defaultProps是组件类本身的属性，用来设置类的默认props。 这是用于未定义的props，但不用于为null的props。 例如：

```javascript
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
};
```
如果没有提供props.color，它将默认设置为“蓝色”：

```javascript
render() {
	return <CustomButton /> ; // props.color will be set to blue
}
```
如果props.color设置为null，那它就是null：

```javascript
  render() {
    return <CustomButton color={null} /> ; // props.color will remain null
  }
```
### displayName
displayName字符串用于调试。 JSX会自动设置此值; 见[深入JSX](../React高级指南/React高级指南（一）深入理解JSX.md)。 

(这篇并没有提到displayName啊，高阶组件里有提到，另外[Context](https://facebook.github.io/react/docs/context.html)中提到的contextTypes和childContextTypes算类属性么？)

## 实例属性

### props
this.props包含由该组件的调用者定义的props。 有关props的介绍，请参阅[组件和props](../React快速开始/React快速开始（四）组件和props.md)。

this.props.children是一个特殊的prop，通常由JSX表达式中的子标签定义，而不是在标签本身		。

特别地，this.props.children是一个特殊的支持，通常由JSX表达式中的子标签定义，而不是标记本身。

### state

state包含该组件的特定数据，该数据可能随时间而改变。 state是用户定义的，它应该是一个纯粹的JavaScript对象。

如果你不在render()中使用它(不用于触发重新渲染)，它不应该放在state中。 例如，您可以直接在实例上保存定时器ID。

有关state的详细信息，请参阅 [state和生命周期](../React快速开始/React快速开始（五）State和生命周期.md)。

不要直接改变this.state，它不会触发render，在调用setState()之后还会覆盖所做的更改。 
