
>[Composition vs Inheritance](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

刚接触React的开发人员通常通过继承来实现代码重用，然而我们建议使用组合而不是继承来重用组件之间的代码。React具有强大的组合模型，可以帮助我们解决这些代码重用的问题，而不需要使用继承。

这篇主要是讲 this.props.children
<!--more-->
## 容器（Containment）
一些组件并不能提前知道他们的children是什么。 比如Sidebar和Dialog组件，他们只表示一个通用的盒子，盒子的内容我们自己定义。

例如下面这样, FancyBorder是一个容器组件，里面展示的内容通过props.children获得

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```
我们可以通过JSX嵌套传递chilren给组件

[Try it on CodePen](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)

```javascript
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

JSX标签&lt;FancyBorder> 内的任何内容都将作为props的children属性值传入FancyBorder组件。 因为FancyBorder在一个&lt;div>内部渲染了{props.children}，所以传递的元素将显示在最终输出中。

有时组件中有多个地方需要显示接收到的props内容，那么你可以这样做：

[Try it on CodePen](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)

```javascript
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

**React元素（例如&lt;Contacts />和&lt;Chat />）都只是对象**，因此可以像其他数据一样作为props传递。

## 特殊化（Specialization）
有时一个组件是另一个组件的“special cases”。 例如，我们可以说WelcomeDialog是Dialog的一个特例。在React中这也可以用组合来实现

[Try it on CodePen](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

组合对于定义为类的组件同样适用：

[Try it on CodePen](https://codepen.io/gaearon/pen/gwZbYa?editors=0010)

```jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }
}

```

## So What About Inheritance?
在Facebook上，有数千个组件使用React，还没有发现任何用例建议使用组件继承。

props和组合已经提供了以明确和安全的方式自定义组件外观和行为所需的所有灵活性。 

**请记住，组件可以接受任意props，包括原始值，React元素或函数。**

如果要在组件之间重用非UI功能，建议将它提取到单独的JavaScript模块中。 
组件可以导入并使用函数，对象或类，而不会扩展它。