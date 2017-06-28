>[Test Utilities](https://facebook.github.io/react/docs/test-utils.html)

# 测试工具
引入：

```
import ReactTestUtils from 'react-dom/test-utils'; // ES6
var ReactTestUtils = require('react-dom/test-utils'); // ES5 with npm
```

## 总览
ReactTestUtils可以让你轻松测试React组件。 在Facebook上，我们使用 [Jest](https://facebook.github.io/jest/) 进行JavaScript测试。 可以通过Jest网站的 [React Tutorial](http://facebook.github.io/jest/docs/tutorial-react.html#content)了解如何开始使用Jest.

Airbnb已经发布了一个名为Enzyme的测试工具，它可以轻松地操作React组件的输出。 如果您决定使用单元测试工具与Jest或任何其他测试程序一起使用，请查看：[http://airbnb.io/enzyme/](http://airbnb.io/enzyme/)

- ```Simulate```
- ```renderIntoDocument()```
- ```mockComponent()```
- ```isElement()```
- ```isElementOfType()```
- ```isDOMComponent()```
- ```isCompositeComponent()```
- ```isCompositeComponentWithType()```
- ```findAllInRenderedTree()```
- ```scryRenderedDOMComponentsWithClass()```
- ```findRenderedDOMComponentWithClass()```
- ```scryRenderedDOMComponentsWithTag()```
- ```findRenderedDOMComponentWithTag()```
- ```scryRenderedComponentsWithType()```
- ```findRenderedComponentWithType()```

## 参考

## Shallow Rendering
在为React编写单元测试程序时，使用浅层渲染可能会有所帮助。 浅层渲染不会对子组件渲染，不必担心未实例化或呈现的子组件的行为。 这也不需要DOM。

>注意：
>浅层渲染方法从``` react-test-renderer/shallow```引入，更多关于浅层渲染的信息参见 [浅层渲染](./React参考（八）浅层渲染.md)

## 其他方法

### Simulate

```javascript
Simulate.{eventName}(
  element,
  [eventData]
)
```

使用可选的eventData事件数据模拟DOM节点上的事件处理。

Simulate对每个React能理解的事件都有一个处理方法。

### 点击一个元素
```javascript
// <button ref="button">...</button>
const node = this.refs.button;
ReactTestUtils.Simulate.click(node);
```

### 更改输入值 然后按ENTER键
```javascript
// <input ref="input" />
const node = this.refs.input;
node.value = 'giraffe';
ReactTestUtils.Simulate.change(node);
ReactTestUtils.Simulate.keyDown(node, {key: "Enter", keyCode: 13, which: 13});
```

注意，您必须提供在组件中使用的任何事件属性（例如keyCode，等等），因为React不会为您创建任何这些属性。

### renderIntoDocument()
```javascript
renderIntoDocument(element)
```

将React元素渲染到DOM节点中,此函数需要一个DOM。

### mockComponent()

```javascript
mockComponent(
  componentClass,
  [mockTagName]
)
```

### isElement()
```javascript
isElement(element)
```
如果element是React元素，则返回true。

### isElementOfType()
```javascript
isElementOfType(
  element,
  componentClass
)
```
如果element的元素类型是componentClass，则返回true。

### isDOMComponent()
```javascript
isDOMComponent(instance)
```
如果instance是DOM组件（如&lt;div> or &lt;span>）,则返回true。

### isCompositeComponent()
```javascript
isCompositeComponent(instance)
```
如果instance是用户自定义的组件（如类组件 or 函数组件）,则返回true。

### isCompositeComponentWithType()
```javascript
isCompositeComponentWithType(
  instance,
  componentClass
)
```
如果instance的组件类型是componentClass，则返回true。

### findAllInRenderedTree()
```javascript
findAllInRenderedTree(
  tree,
  test
)
```
遍历树中的所有组件，并获取test(component)为true的所有组件。 这主要用作其他测试工具的原始数据。

### scryRenderedDOMComponentsWithClass()
```javascript
scryRenderedDOMComponentsWithClass(
  tree,
  className
)
```
查找渲染树中所有className 为给定className 的DOM元素。

### findRenderedDOMComponentWithClass()
```javascript
findRenderedDOMComponentWithClass(
  tree,
  className
)
```

与scryRenderedDOMComponentsWithClass()一样，但是只能有一个结果匹配，并返回这个结果。如果有多个结果，则会抛出异常。

### scryRenderedDOMComponentsWithTag()
```javascript
scryRenderedDOMComponentsWithTag(
  tree,
  tagName
)
```
查找渲染树中所有标签名 为给定tagName 的DOM元素。

### findRenderedDOMComponentWithTag()
```javascript
findRenderedDOMComponentWithTag(
  tree,
  tagName
)
```
与scryRenderedDOMComponentsWithTag()一样，但是只能有一个结果匹配，并返回这个结果。如果有多个结果，则会抛出异常。

### scryRenderedComponentsWithType()
```javascript
scryRenderedComponentsWithType(
  tree,
  componentClass
)
```
查找所有类型是给定的componentClass的组件实例

### findRenderedComponentWithType()
```javascript
findRenderedComponentWithType(
  tree,
  componentClass
)
```
与findRenderedComponentWithType()一样，但是只能有一个结果匹配，并返回这个结果。如果有多个结果，则会抛出异常。