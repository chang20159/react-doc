
>[Handling Events](https://facebook.github.io/react/docs/handling-events.html)

# 事件处理
React元素事件处理与DOM元素上的事件处理很相似，但有一些语法差异：

- React事件绑定采用驼峰式命名（onClick），而不是小写(onclick)。
- 使用JSX，你可以传递一个函数作为事件处理程序，而不是一个字符串。


例如，html中绑定事件

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```
在React中略有不同

```html
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

**另一个区别是:在React中不能通过return false来阻止默认行为，必须显式调用preventDefault。** 

> 还有在react中onClick最终要绑定在DOM元素上才有效

例如，为了阻止链接打开新页面的默认行为，在纯HTML中可以这样写：

```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```
在React中，必须这样写：

```javascript
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
**这里的e对象是一个合成的event, React根据[W3C事件规范](https://www.w3.org/TR/DOM-Level-3-Events/)定义了这些合成event，因此你不必担心跨浏览器的兼容性。
请参阅[SyntheticEvent参考指南](https://facebook.github.io/react/docs/events.html)了解更多信息。**

使用React时，通常不需要在创建DOM元素之后调用addEventListener来添加监听器，在元素最初呈现时就可以提供一个监听器。（React采用事件代理机制）

当使用ES6类定义组件时，常见的做法是将事件处理程序作为类方法。 

例如，这个Toggle组件有一个按钮，让用户在“ON”和“OFF”状态之间切换：
[Try it on CodePen](https://codepen.io/gaearon/pen/xEmzGg?editors=0010)

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

在JSX回调中必须要注意一点: 
- 在JavaScript中，类方法默认情况下是不会[绑定](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)this的， 如果没有给this.handleClick绑定上下文就传递给onClick，那么当该函数实际被调用时，函数内的this是undefined的。

这并不是React的特定行为，它是[JavaScript函数如何工作](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)的一部分。通常，如果你引用一个没有()的方法，比如onClick = {this.handleClick}，你应该给该方法绑定上下文。

如果觉得调用bind函数比较麻烦，还有两种方法可以绑定this

1、 属性初始化器 [property initializer syntax](https://babeljs.io/docs/plugins/transform-class-properties/)

```javascript
class LoggingButton extends React.Component {
  // 这个语法能够handleClick中的`this`被绑定。
  // 注意: 这是一个实验性语法
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```
默认情况下，此语法在[ Create React App](https://github.com/facebookincubator/create-react-app)中启用。

2、[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```javascript
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```
这种办法有个问题就是，每次LoggingButton渲染都会重新创建一个回调函数。多数情况下还好，如果这个回调函数作为props传递给下面的子组件，可能会让这些组件进行额外的重新渲染。

>箭头函数的引入有两个方面的作用：一是更简短的函数书写，二是对 this的词法解析。
>箭头函数会捕获其所在上下文的  this 值，作为自己的 this 值

通常建议在构造函数中绑定或使用属性初始化器语法[property initializer syntax]来避免这种性能问题。
