>[React Top-Level API](https://facebook.github.io/react/docs/react-api.html)


# React顶级API
React是React库的入口点。 

- 如果从&lt;script>标签加载React，则在React上的顶级API可以全局使用。 
- 如果使用ES6，可以```import React from 'react'```
- 如果使用ES5，可以```var React = require（'react'）```

## API总览

### 组件
React组件可以让我们将UI拆分为独立、可重复使用的部分，并可分别考虑每个部分。 React组件可以通过React.Component或React.PureComponent定义。

- ```React.Component```
- ```React.PureComponent```

如果不使用ES6类，可以使用create-react-class模块。 

更多详细信息，请参阅 [不使用ES6的React](../React高级指南/React高级指南（六）不使用ES6的React.md)
### 创建一个React元素
我们建议使用JSX来描述UI的外观。 每个JSX元素只是调用React.createElement()的语法糖。 如果您使用JSX，通常不会直接调用以下方法。

- ```createElement()```
- ```createFactory()```

更多详细信息，请参阅 [不使用JSX的React](../React高级指南/React高级指南（七）不使用JSX的React.md)

### 转换元素
React也提供了一些其他的API：

- ```cloneElement()```
- ```isValidElement()```
- ```React.Children```

## 参考

### React.Component
React.Component是使用 [ES6类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes) 定义React组件的基类。

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
有关基类React.Component的方法和属性的列表，请参阅[React.Component API](https://facebook.github.io/react/docs/react-component.html)。


### React.PureComponent
React.PureComponent与 [React.Component](#React.Component)完全相同，但实现了 [shouldComponentUpdate()](https://facebook.github.io/react/docs/react-component.html#shouldcomponentupdate)，在其中对prop、state进行了浅比较。

如果您的React组件的render()方法在相同的prop和state下呈现相同的结果，则可以在某些情况下使用React.PureComponent提升性能。

>注意：React.PureComponent的shouldComponentUpdate()只是对对象进行浅比较，如果包含复杂的数据结构，对于深层次的数据变化会判定为无变化。 只有当prop和state比较简单时才能extend PureComponent，或者在知道深层数据结构已更改时使用 [forceUpdate()](../React参考指南/React参考（二）React.Component.md#forceupdate)。 或者使用[不可变对象](https://facebook.github.io/immutable-js/)对嵌套数据进行快速比较。

>此外，React.PureComponent的shouldComponentUpdate() 将跳过整个组件子树的prop更新。 

### createElement()

```javascript
React.createElement(
  type,
  [props],
  [...children]
)
```
创建并返回一个给定类型的React元素，type可以是一个字符串标签（如'div' or 'span'），也可以是React组件（类或函数）。

React.DOM提供了对于DOM调用React.createElement(）的封装。例如React.DOM.a(...) 是对React.createElement('a', ...)的一个简单的封装。但这种用法已经被弃用了，我们建议使用JSX或直接使用React.createElement()。

用JSX编写的代码将被转换为React.createElement()。 如果您使用JSX，通常不用直接调用React.createElement()。 

请参阅 [React Without JSX](../React高级指南/React高级指南（七）不使用JSX的React.md)了解更多信息。

### cloneElement()

```javascript
React.cloneElement(
  element,
  [props],
  [...children]
)
```
克隆并使用元素作为起点返回一个新的React元素。 所产生的元素将具有原始元素的prop，与新的prop浅合并。 新children将取代现有的children。 原始元素中的key和ref将被保留。

React.cloneElement() 等同于：

```xml
<element.type {...element.props} {...props}>{children}</element.type>
```
这个API的引入是为了取代不推荐使用的React.addons.cloneWithProps()

### createFactory()

```javascript
React.createFactory(type)
```
返回一个创建给定类型React元素的函数。 像React.createElement()一样，type参数可以是字符串标记（例如'div'或'span'），也可以是React组件类型（类或函数）。

这个API被废弃了，我们建议使用JSX或直接使用React.createElement()。

如果您使用JSX，通常不会直接调用React.createFactory()。 

请参阅 [React Without JSX](../React高级指南/React高级指南（七）不使用JSX的React.md)了解更多信息。

### isValidElement()
```javascript
React.isValidElement(object)
```
验证有个对象是不是React元素。 返回true或false

### React.Children
React.Children提供了处理this.props.children不透明数据结构（数据结构是不确定的）的方法。

#### React.Children.map

```javascript
React.Children.map(children, function[(thisArg)])
```

遍历子元素，映射为一个新的子元素集合.如果children是null或undefined，返回的也是null或undefined。

#### React.Children.forEach

```javascript
React.Children.forEach(children, function[(thisArg)])
```
遍历子元素，对每一个子元素执行回调函数，不会返回新的元素集合。
#### React.Children.count
```javascript
React.Children.count(children)
```

返回children中组件的总数，与map或forEach的回调调用次数相同。

#### React.Children.only

```javascript
React.Children.only(children)
```
返回仅有的一个子元素，否则（没有子元素或超过一个子元素）报错.

#### React.Children.toArray

```javascript
React.Children.toArray(children)
```

将children不透明数据结构作为平面数组返回，并将key分配给每个child。 这对于想要在render方法中操作children的集合很有用，特别是如果要在传递给render()之前对this.props.children重新排序或切片。