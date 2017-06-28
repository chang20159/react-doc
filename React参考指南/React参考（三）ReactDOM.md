>[ReactDOM](https://facebook.github.io/react/docs/react-dom.html)

# ReactDOM
- 如果从&lt;script>标签加载React，则这些在ReactDOM上的顶级API可在全局中使用。 
- 如果您使用ES6，可以写```import ReactDOM from 'react-dom'```
- 如果你使用ES5，可以写```var ReactDOM = require（'react-dom'）```

## 总览
react-dom提供特定的DOM方法，可以在应用程序的顶层使用。如果需要，可以在React模型之外使用。 大多数组件不需要使用此模块。

- ```render()```
- ```unmountComponentAtNode()```
- ```findDOMNode()```

### 浏览器支持
React支持所有流行的浏览器，包括Internet Explorer 9及更高版本。

>注意，对于不支持ES5方法的旧版浏览器，React不支持。但如果应用程序包含es5-shim和es5-sham等polyfill，React也可以在旧版浏览器中正常工作。 

## 参考
### render()
```javasript
ReactDOM.render(
  element,
  container,
  [callback]
)
```
将React元素渲染到container的DOM中，并返回对组件的 [引用](../React高级指南/React高级指南（三）Refs与DOM.md)（或对于 [无状态组件](../React快速开始/React快速开始（四）组件和props.md) 返回null）。

如果React元素先前已经渲染到container中，那么ReactDOM.render将对其进行更新，并且只做有必要的DOM更新，来呈现最新的React元素。

如果提供了可选的回调，它将在组件呈现或更新后执行。

>ReactDOM.render()控制着你传入的container节点的内容。当第一次调用时，任何现有的DOM元素都将被替换。 后面调用ReactDOM.render()则使用React的DOM diffing算法进行有效更新。
>
>ReactDOM.render()不会修改container节点（仅修改container的子节点）。 可能会将组件插入到现有的DOM节点，而不会覆盖现有的子节点。
>
>当前ReactDOM.render()返回对ReactComponent实例的引用。 但是，应该避免使用此返回值，这种做法将被废弃，因为React的未来版本在某些情况下可能异步呈现组件。 如果您需要对根React组件实例的引用，首选解决方案是将 [引用回调](../React高级指南/React高级指南（三）Refs与DOM.md) 附加到根元素。

### unmountComponentAtNode()
```javasript
ReactDOM.unmountComponentAtNode(container)
```
从DOM中删除已装载的React组件，并清理其事件处理程序和state。 如果容器中没有已安装组件，调用此函数不会做任何事情。 如果组件已卸载，则返回true，如果没有组件卸载，则返回false。
### findDOMNode()
```javasript
ReactDOM.findDOMNode(component)
```

如果组件已经被安装到DOM中，则返回相应的浏览器原生的DOM元素。 此方法对于从DOM读取值非常有用，例如表单字段值和执行DOM测量。 **在大多数情况下，您可以附加一个ref到DOM节点，并避免使用findDOMNode。** 当render返回null或false时，findDOMNode返回null。

>findDOMNode是一个用于访问底层DOM节点的最后方案。 在大多数情况下，不鼓励使用此方法，因为它打破了组件抽象。
>
>findDOMNode仅适用于已安装的组件（即已放置在DOM中的组件）。 如果您尝试在尚未挂载的组件上调用此函数（例如在render()方法中尚未创建的组件上调用findDOMNode()），将抛出异常。
>
>findDOMNode不能用于函数组件。