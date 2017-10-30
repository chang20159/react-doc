>[Portals](https://reactjs.org/docs/portals.html)

Portals提供了一种方法，可以将子节点挂载在父组件的DOM层次结构之外的DOM节点中。

```javascript
ReactDOM.createPortal(child, container)
```

- 第一个参数child可以是任何一个可渲染的React的child，例如元素、字符串或者片段。
- 第二个参数container是一个DOM元素

## 用法

通常情况下，当你从组件的render方法中返回一个元素时，它会作为最近父节点的子元素装载到DOM中：

```javascript
render() {
  // React mounts a new div and renders the children into it
  return (
    <div>
      {this.props.children}
    </div>
  );
}
```

然而有时候我们需要将一个子元素挂载到文档树中的另外一个位置:

```javascript
render() {
  // React不会创建一个新的div, 它会将children渲染到 `domNode`中.
  // `domNode` 可以是任何一个有效的 DOM 节点, 不管它在DOM树中的什么位置.
  return ReactDOM.createPortal(
    this.props.children,
    domNode,
  );
}
```

Portals一个典型的用法是：像dialogs, hovercards, and tooltips这种弹出类UI，父组件一般有`overflow: hidden`或`z-index`的样式，但它的child需要弹出显示。

>一定要记住，在使用Portals时要保证适当的可访问性。


[Try it on CodePen](https://codepen.io/gaearon/pen/yzMaBd)

```xml
<div id="app-root"></div>
<div id="modal-root"></div>
```

```javascript
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }
  
  componentDidMount() {
    modalRoot.appendChild(this.el);
  }
  
  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }
  
  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el,
    );
  }
}
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showModal: false};
    
    this.handleShow = this.handleShow.bind(this);
    this.handleHide = this.handleHide.bind(this);
  }

  handleShow() {
    this.setState({showModal: true});
  }
  
  handleHide() {
    this.setState({showModal: false});
  }

  render() {
    const modal = this.state.showModal ? (
      <Modal>
        <div className="modal">
          <div>
            With a portal, we can render content into a different
            part of the DOM, as if it were any other React child.
          </div>
          This is being rendered inside the #modal-container div.
          <button onClick={this.handleHide}>Hide modal</button>
        </div>
      </Modal>
    ) : null;

    return (
      <div className="app">
        This div has overflow: hidden.
        <button onClick={this.handleShow}>Show modal</button>
        {modal}
      </div>
    );
  }
}

ReactDOM.render(<App />, appRoot);
```

下面是Modal组件使用portal和不使用portal渲染后最终在dom树中的位置：

<div style="display:flex;display:-webkit-flex;">
	<img src="../image/not_use_ portal.png" width="50%"/><img src="../image/use_ portal.png" width="50%"/>
</div>

- 左边没有使用portal，model装载在react元素树中对应的位置
- 右边没有使用portal，model装载在指定的id为modal-root的dom元素中

## Portals上的事件冒泡

虽然portal可以装载在dom树中的任何一个位置，但它的行为还是跟正常的React元素一样。例如React的context特性，不管是否是portal，也不管这个portal装载在dom树中的什么位置，因为它还在React元素树中。

这也包括事件冒泡。从portal内部触发的事件将传播到所在元素树中的祖先，即使这些元素不是DOM树中的祖先元素。例如有下面的HTML结构：

```xml
<html>
  <body>
    <div id="app-root"></div>
    <div id="modal-root"></div>
  </body>
</html>
```

`#app-root`中的`Parent`组件能够捕获到兄弟节点`#modal-root`中的冒泡事件。

[Try it on CodePen](https://codepen.io/gaearon/pen/jGBWpE)

```javascript
//这两个containers在 DOM树中是兄弟节点
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el,
    );
  }
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {clicks: 0};
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // This will fire when the button in Child is clicked,
    // updating Parent's state, even though button
    // is not direct descendant in the DOM.
    this.setState(prevState => ({
      clicks: prevState.clicks + 1
    }));
  }

  render() {
    return (
      <div onClick={this.handleClick}>
        <p>Number of clicks: {this.state.clicks}</p>
        <p>
          Open up the browser DevTools
          to observe that the button
          is not a child of the div
          with the onClick handler.
        </p>
        <Modal>
          <Child />
        </Modal>
      </div>
    );
  }
}

function Child() {
  // The click event on this button will bubble up to parent,
  // because there is no 'onClick' attribute defined
  return (
    <div className="modal">
      <button>Click</button>
    </div>
  );
}

ReactDOM.render(<Parent />, appRoot);
``` 
允许父组件捕获它内部portal中的冒泡事件，这样可以让我们更加灵活的开发。例如如果要呈现一个&lt;Modal /> 组件,父组件在捕获事件时，不需要关心是否使用了portal。

## 翻译总结

Portals是React 16提供的官方解决方案，使得组件可以脱离父组件层级挂载在DOM树的任何位置。主要应用于弹出类UI。

那为什么要给出Portals，是为了解决什么问题呢？

对于弹出层类的UI，通常叫做modal，通常会显示在相对于窗口的某个位置，如果挂载在某个组件中，容易出现[Stacking Context](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context)等样式影响，所以通常会将modal直接添加到body元素下。Portals正是解决了这个问题。

