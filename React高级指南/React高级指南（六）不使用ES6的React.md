>[React Without ES6](https://facebook.github.io/react/docs/react-without-es6.html)

# 不使用ES6的React
通常我们会将React组件定义为一个JavaScript类：

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
如果不使用ES6，也可以使用create-react-class模块：

```javascript
var createReactClass = require('create-react-class');
var Greeting = createReactClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});

```

ES6类的API与createReactClass()类似，但也有一些不同。

## 声明 Default Props
对于函数组件和类组件，defaultProps被定义为组件本身的属性：

```javascript
class Greeting extends React.Component {
  // ...
}

Greeting.defaultProps = {
  name: 'Mary'
};

```
使用createReactClass()，您需要将getDefaultProps()定义为传递对象上的函数：

```javascript
var Greeting = createReactClass({
  getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },

  // ...
});

```
## 设置 Initial State
在ES6类中，您可以在构造函数中设置this.state来定义初始状态：

```javascript
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }
  // ...
}

```

使用createReactClass()，您必须提供一个返回初始状态的方法getInitialState：

```javascript
var Counter = createReactClass({
  getInitialState: function() {
    return {count: this.props.initialCount};
  },
  // ...
});
```
## 自动绑定this
在声明为ES6类的React组件中，方法遵循与常规ES6类相同的语义，这意味着React不会自动将其绑定到实例。 你必须在构造函数中明确使用.bind（this）：


```javascript
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
    // 这一行很重要!！
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    alert(this.state.message);
  }

  render() {
    // 因为 `this.handleClick` 已经绑定到实例, 我们可以直接使用它作为事件处理程序
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}

```
使用createReactClass()，就不需要了，因为它绑定了所有方法：

```javascript
var SayHello = createReactClass({
  getInitialState: function() {
    return {message: 'Hello!'};
  },

  handleClick: function() {
    alert(this.state.message);
  },

  render: function() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
});
```

这意味着使用ES6类会让事件处理程序有很多重复代码，如this.xxx.bind(this)。如果你不喜欢代码中有很多bind，可以借助Babel使用提案中的[实验性类属性](https://babeljs.io/docs/plugins/transform-class-properties/) [属性初始化器](https://babeljs.io/docs/plugins/transform-class-properties/)

```javascript
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
  }
  // 注意: 这个语法是实验性的!
  // 使用箭头绑定方法:
  handleClick = () => {
    alert(this.state.message);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

请注意，上面的语法是实验性的，语法可能会改变，或者提案可能不会使其成为语言规范。

有几种安全的做法：

- 在构造函数中绑定方法
- 使用箭头函数，例如 onClick = {（e）=> this.handleClick（e）}。
- 使用createReactClass

## Mixins

>注意，ES6不支持mixin。 因此，当您使用React与ES6类时，不支持mixins。		
>我们还在使用mixins的代码库中发现了许多问题，**不建议在新代码中使用它们**。		
>本节仅供参考，不要应用于项目中

有时完全不同的组件可能会共享一些常见的功能。 这种情况有时被称为[ cross-cutting concerns](https://en.wikipedia.org/wiki/Cross-cutting_concern)，为此可以让您可以通过createReactClass使用老的mixins系统来实现。

一个常见的例子是希望每个一段时间更新一次组件，你会很容易想到setInterval()，但有一点很重要：当你不需要时要去取消这个定时功能，以节省内存。

React提供了生命周期方法，让您知道何时创建或销毁组件。我们可以创建一个简单的mixin:用这些方法来提供一个简单的setInterval() 函数，当组件销毁时，这个函数会自动清除。

```javascript
var SetIntervalMixin = {
  componentWillMount: function() {
    this.intervals = [];
  },
  setInterval: function() {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function() {
    this.intervals.forEach(clearInterval);
  }
};

var createReactClass = require('create-react-class');

var TickTock = createReactClass({
  mixins: [SetIntervalMixin], // Use the mixin
  getInitialState: function() {
    return {seconds: 0};
  },
  componentDidMount: function() {
    this.setInterval(this.tick, 1000); // Call a method on the mixin
  },
  tick: function() {
    this.setState({seconds: this.state.seconds + 1});
  },
  render: function() {
    return (
      <p>
        React has been running for {this.state.seconds} seconds.
      </p>
    );
  }
});

ReactDOM.render(
  <TickTock />,
  document.getElementById('example')
);

```

如果一个组件正在使用多个mixins，并且多个mixin定义了相同的生命周期方法（例如当组件被销毁时，几个mixin都要进行一些清理工作），所有的声明周期方法都会执行。定义在mixins中的方法会按列出的mixins的顺序执行。例如两个mixin都有componentWillMount方法，在组件装载之前会先执行前面的mixin中的componentWillMount

## 总结

'不使用ES6的React'，应该叫create-react-class的用法