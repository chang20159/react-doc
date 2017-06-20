>[Web Components](https://facebook.github.io/react/docs/web-components.html)

# Web Components
[Web组件](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components) （建议先看下Web Components的介绍）为可重用组件提供了强大的封装，而React是一个JavaScript库，用于构建UI，使DOM与数据保持同步。他们解决的问题不同，但解决目标是互补的。 作为开发人员，您可以在Web Components中自由使用React，或者在React中使用Web Components，或者同时使用两者。
## 在React中使用Web Components

```javascript
class HelloMessage extends React.Component {
  render() {
    return <div>Hello <x-search>{this.props.name}</x-search>!</div>;
  }
}
```

注意：

- Web组件通常会暴露一个API。 例如，视频Web组件可能会提供play()和pause()函数。 
- 要访问Web组件的API，需要使用ref直接与DOM节点进行交互。 
- 如果使用第三方Web组件，最佳解决方案是编写一个React组件，把Web组件包在里面。
- Web组件触发的事件可能无法通过React渲染树正确传播，需要在React组件内部手动添加事件处理程序。

还要注意一点：Web Components使用“class”而不是“className”。

```javascript
function BrickFlipbox() {
  return (
    <brick-flipbox class="demo">
      <div>front</div>
      <div>back</div>
    </brick-flipbox>
  );
}

```

## 在你的Web Components中使用React

```
const proto = Object.create(HTMLElement.prototype, {
  attachedCallback: {
    value: function() {
      const mountPoint = document.createElement('span');
      this.createShadowRoot().appendChild(mountPoint);

      const name = this.getAttribute('name');
      const url = 'https://www.google.com/search?q=' + encodeURIComponent(name);
      ReactDOM.render(<a href={url}>{name}</a>, mountPoint);
    }
  }
});
document.registerElement('x-search', {prototype: proto});
```

PS:  这篇的应用见得比较少。。。
