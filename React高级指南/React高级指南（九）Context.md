>[Context](https://facebook.github.io/react/docs/context.html)

# Context
使用React可以轻松地通过React组件跟踪数据流。 当你看到一个组件，就能知道props被传递。

有时候希望通过组件树传递数据，但又不想在每个级别都编写代码传递props。 我们可以使用强大的“context”API直接在React中执行此操作。

## 为什么不使用Context
绝大多数应用程序不需要使用Context,如果您希望应用程序稳定，请不要使用上下文Context。 **这是一个实验性API，将来的React版本不一定支持**。

如果您不熟悉[Redux](https://github.com/reactjs/redux)或[MobX](https://github.com/mobxjs/mobx)等状态管理库，请不要使用上下文。 对于许多实际应用，这些库及其与React的绑定库（如react-redux）是管理与许多组件相关联的状态的不错选择。 您的正确解决方案更可能是Redux，而不是Context。

如果您不是有经验的React开发人员，请不要使用Context。 通常使用props和state是实现功能的更好的方法。

如果您坚持使用Context，请在小范围内使用Context，尽可能的避免直接使用Context  API，以便当API更改时更容易升级。

## 怎样使用Context
这里有一个组件树：

```javascript
class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.props.color}}>
        {this.props.children}
      </button>
    );
  }
}

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button color={this.props.color}>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  render() {
    const color = "purple";
    const children = this.props.messages.map((message) =>
      <Message text={message.text} color={color} />
    );
    return <div>{children}</div>;
  }
}
```
在上面这个例子中，在MessageList中设置color,然后手动将color通过props向下传递至Button，穿过了组件树 MessageList > Message > Button。

使用Context，可以自动在组件树中传递：

```javascript
const PropTypes = require('prop-types');

class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.context.color}}>
        {this.props.children}
      </button>
    );
  }
}

Button.contextTypes = {
  color: PropTypes.string
};

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  getChildContext() {
    return {color: "purple"};
  }

  render() {
    const children = this.props.messages.map((message) =>
      <Message text={message.text} />
    );
    return <div>{children}</div>;
  }
}

MessageList.childContextTypes = {
  color: PropTypes.string
};
```

给MessageList（Context提供者）添加**childContextTypes**属性，并给MessageList实例添加**getChildContext**方法，React会自动的将context信息（color）向下传递给子树，添加了**contextTypes**的子组件可以通过this.context获得祖先传递下来的数据，如this.context.color。

**如果没有定义contextTypes，this.context是一个空对象**

## 父组件与子组件通信
还可以通过Context构建一个父组件和子组件通信的API。 例如，[React Router V4](https://reacttraining.com/react-router/)的原理就是这样：

```javascript
import { BrowserRouter as Router, Route, Link } from 'react-router-dom';

const BasicExample = () => (
  <Router>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>

      <hr />

      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/topics" component={Topics} />
    </div>
  </Router>
);
```
可以研究一下react-router的实现原理。

## 在生命周期方法中使用Context
如果给组件定义了contextTypes，则以下 [生命周期方法](../React参考指南/React参考（二）React.Component.md) 将接收一个附加参数，即context对象：

- `constructor(props, context)`
- `componentWillReceiveProps(nextProps, nextContext)`
- `shouldComponentUpdate(nextProps, nextState, nextContext)`
- `componentWillUpdate(nextProps, nextState, nextContext)`
- `componentDidUpdate(prevProps, prevState, prevContext)`

## 在无状态函数组件中使用Context
如果给函数组件也定义了contextTypes属性，那在无状态函数组件中也可以使用Context。
下面将Button组件声明为函数：

```javascript
const PropTypes = require('prop-types');

const Button = ({children}, context) =>
  <button style={{background: context.color}}>
    {children}
  </button>;

Button.contextTypes = {color: PropTypes.string};
```
注意函数组件的第一个参数是props，这里使用了对象解构。

## 更新Context
不要这样做。

React有一个API来更新上下文，但是它基本上被推翻了，你不应该使用它。

当state或props更改时，会调用getChildContext函数。 为了更新Context中的数据，请使用this.setState触发本地状态更新。 这将产生一个新的context，并被children接收。

```javascript
const PropTypes = require('prop-types');

class MediaQuery extends React.Component {
  constructor(props) {
    super(props);
    this.state = {type:'desktop'};
  }

  getChildContext() {
    return {type: this.state.type};
  }

  componentDidMount() {
    const checkMediaQuery = () => {
      const type = window.matchMedia("(min-width: 1025px)").matches ? 'desktop' : 'mobile';
      if (type !== this.state.type) {
        this.setState({type});
      }
    };

    window.addEventListener('resize', checkMediaQuery);
    checkMediaQuery();
  }

  render() {
    return this.props.children;
  }
}

MediaQuery.childContextTypes = {
  type: PropTypes.string
};
```

问题是，当组件提供的context的值更改时，使用该值的后代的中间父组件在调用shouldComponentUpdate时返回false，就不会更新了，也就完全不能控制组件，所以基本上没有办法可靠地更新context。 这篇博客 [怎样安全的使用react context](https://medium.com/@mweststrate/how-to-safely-use-react-context-b7e343eff076) 很好的解释了：为什么这是一个问题，以及如何解决它。

**疑问： 如果手动传递props，在这种情况下不是也不能更新么么么？？**




