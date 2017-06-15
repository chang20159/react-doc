
>[Components and Props](https://facebook.github.io/react/docs/components-and-props.html)

使用组件可以将UI拆分成独立的可重复使用的部分，然后可以单独考虑每个组件的渲染。从概念上来讲，组件就像JavaScript函数，接受任意输入（称为“props”），并返回描述页面呈现的React元素。
<!-- more -->

### 函数组件和类组件（Functional and Class ）
定义组件最简单的方法是：编写一个JavaScript函数：
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

这个函数是一个有效的React组件，因为它接受一个单一的“props”对象参数并返回一个React元素。 我们将这样的组件称为“functional组件”，因为它们在字面上是一个JavaScript函数。

还可以使用[ES6类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes)来定义组件：
```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
上面这两个组件是等同的，class组件有一些额外的功能（state、钩子函数），这会在下一章节讨论，这里先使用functional组件来讨论

### 渲染组件
之前我们遇到的元素，都是DOM标签：
```javascript
const element = <div />;
```

我们也可以自定义元素，代表一个组件
```javascript
const element = <Welcome name="Sara" />;
```
当React知道这是一个表示用户定义的元素时，它将JSX属性作为单个对象传递给该组件。 我们称这个对象为“props”。例如：

[Try it on CodePen](https://codepen.io/gaearon/pen/YGYmEG?editors=0010)
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
我们来看一下这个例子发生了什么？

- 调用ReactDOM.render()渲染&lt;Welcome name =“Sara”/&gt;元素
- React以{name：'Sara'}作为props调用Welcome组件
- Welcome组件返回一个&lt;h1&gt; Hello，Sara &lt;/ h1&gt;元素
- React DOM有效地更新DOM来匹配&lt;h1&gt; Hello，Sara &lt;/ h1&gt; 

**注意：组件名称要以大写字母开头，例如，&lt;div /&gt;表示一个DOM标签，但&lt;Welcome /&gt;表示一个组件，并且要求Welcome在作用范围内（在本模块中或从其他模块引入）。**

我们可以创建一个组件，然后呈现多个
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
**注意：组件必须返回单个根元素，这也是为什么我们添加了一个&lt;div&gt;来包含所有的&lt;Welcome /&gt;元素。**

### props是只读的
不管将组件声明为functional还是class，它都不能修改自己的props。 
看下面这个函数，这样的函数称为‘纯函数’，因为它们不会更改输入，并且相同的输入总是返回相同的结果。

```javascript
function sum(a, b) {
  return a + b;
}
```
相比之下，这个函数是不纯的，因为它改变了自己的输入：
```javascript
function withdraw(account, amount) {
  account.total -= amount;
}
```

再看下组件声明：
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
React很灵活，但有一个严格的规则：**所有的React组件必须像纯函数一样不能改变props**
当然，应用程序的UI是动态的，在下一节中，我们将介绍“state”，state允许React组件根据用户操作、网络响应或者其他任何内容来更改组件输出，而不会违反此规则。