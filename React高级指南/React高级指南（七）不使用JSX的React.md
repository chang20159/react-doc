>[React Without JSX](https://facebook.github.io/react/docs/react-without-jsx.html)

# 不使用JSX的React
JSX并不是使用React所必须的， 如果不想在构建环境中编译JSX时，可以不使用JSX，这样也很方便。


每个JSX元素都只是调用React.createElement(component，props，... children)的语法糖，所以可以用JSX做的事情也可以用简单的JavaScript来完成。

例如，使用JSX的代码：

```javascript
class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.toWhat}</div>;
  }
}

ReactDOM.render(
  <Hello toWhat="World" />,
  document.getElementById('root')
);

```
会被编译成：

```javascript
class Hello extends React.Component {
  render() {
    return React.createElement('div', null, `Hello ${this.props.toWhat}`);
  }
}

ReactDOM.render(
  React.createElement(Hello, {toWhat: 'World'}, null),
  document.getElementById('root')
);

```

注意：Class也会被编译的，这里只展示了JSX编译的结果

如果想知道JSX如何转换为JavaScript，可以使用 [在线Babel编译器](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-0&targets=&browsers=&builtIns=false&debug=false&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyA)。

React.createElement(component，props，... children)中的component可以是一个字符串（DOM元素），可以是类组件，也可以是一个无状态的函数组件。

如果不想写React.createElement这么多，可以像这样：

```javascript
const e = React.createElement;

ReactDOM.render(
  e('div', null, 'Hello World'),
  document.getElementById('root')
);

```

或者，你可以参考社区项目 [react-hyperscript](https://github.com/mlmorg/react-hyperscript) 和 [hyperscript-helpers](https://github.com/ohanhi/hyperscript-helpers)，它们提供更简洁的语法。

