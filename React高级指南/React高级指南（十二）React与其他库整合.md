>[Integrating with Other Libraries](https://facebook.github.io/react/docs/integrating-with-other-libraries.html)


# 与其他库集成
React可用于任何Web应用程序，可以嵌入到其他应用程序中，其他应用程序也可以嵌入到React中。 本篇将介绍一些常见的用例，主要是React与 [jQuery](https://jquery.com/)和 [Backbone](http://backbonejs.org/)的集成，这里介绍的思路也可用于与其他库的集成。
## 与DOM操作插件集成
React之外的库对DOM的更改，React本身并不知道，它仅基于自己的内部表示来确定是否更新，如果相同的DOM节点被另一个库操作，则React会很迷惑。

但这并不意味着将React与操作DOM的其他方式相结合是不可能的事情，，你只需要关注它们各自在做什么。

避免冲突的最简单方法是阻止React组件更新，我们可以通过渲染React无法更新的元素来实现，如空的&lt;div />。


### 如何解决问题
我们来编写一个通用的jQuery插件的包装器：

```javascript
class SomePlugin extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.somePlugin();
  }

  componentWillUnmount() {
    this.$el.somePlugin('destroy');
  }

  render() {
    return <div ref={el => this.el = el} />;
  }
}
```

在根DOM元素上添加[ref](React高级指南/React高级指南（三）Refs与DOM.md)属性，获取DOM节点的引用。 在componentDidMount中，把它传递给jQuery插件。

为了防止React在安装后更新DOM，我们从render()方法返回一个空的&lt;div />。 &lt;div />元素没有属性或子元素，所以React不会更新它，这样jQuery插件可以自由地管理DOM的一部分。

注意，我们同时定义了componentDidMount和componentWillUnmount [生命周期钩子](https://facebook.github.io/react/docs/react-component.html#the-component-lifecycle)。 许多jQuery插件会将事件侦听器添加到DOM，因此在componentWillUnmount中从DOM上去除监听器很重要。 如果插件没有提供清理方法，您可能需要提供自己的方法，记住删除任何注册事件侦听器，防止内存泄漏。


### 与jQuery选择插件集成
>注意：这里只是向您介绍这种可能性，并不意味着它是React应用程序的最佳方法。我们鼓励您可以使用React组件。React组件在React应用程序中更容易重用，并且通常可以更好地控制其行为和外观。

下面介绍一个更具体的例子：为 jQuery插件[Chosen](https://harvesthq.github.io/chosen/)写一个最小的包装器，它增加了&lt;select>的输入。

[Try it on CodePen](https://codepen.io/gaearon/pen/xdgKOz?editors=0010)

```javascript
class Chosen extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.chosen();

    this.handleChange = this.handleChange.bind(this);
    this.$el.on('change', this.handleChange);
  }
  
  componentDidUpdate(prevProps) {
    if (prevProps.children !== this.props.children) {
      this.$el.trigger("chosen:updated");
    }
  }

  componentWillUnmount() {
    this.$el.off('change', this.handleChange);
    this.$el.chosen('destroy');
  }
  
  handleChange(e) {
    this.props.onChange(e.target.value);
  }

  render() {
    return (
      <div>
        <select className="Chosen-select" ref={el => this.el = el}>
          {this.props.children}
        </select>
      </div>
    );
  }
}

function Example() {
  return (
    <Chosen onChange={value => console.log(value)}>
      <option>vanilla</option>
      <option>chocolate</option>
      <option>strawberry</option>
    </Chosen>
  );
}

ReactDOM.render(
  <Example />,
  document.getElementById('root')
);
```
## 与其他UI库集成
因为 [ReactDOM.render()](https://facebook.github.io/react/docs/react-dom.html#render) 比较灵活，React可以嵌入到其他应用程序中。

虽然React在启动时通常是将单个根React组件加载到DOM中，但也可以为UI的独立模块（可以小到一个按钮，大到一个应用程序）调用ReactDOM.render()多次。
### 用React替换基于字符串的渲染
在旧版Web应用程序中的常见模式是将DOM块描述为字符串，并将其插入到DOM中，如：$ el.html(htmlString)。 在这些地方可以引入React，只需将基于字符串的渲染重写为React组件。

所以下面的jQuery实现：

```javascript
$('#container').html('<button id="btn">Say Hello</button>');
$('#btn').click(function() {
  alert('Hello!');
});
```

可以用 React 组件重写成：

```javascript
function Button() {
  return <button id="btn">Say Hello</button>;
}

ReactDOM.render(
  <Button />,
  document.getElementById('container'),
  function() {
    $('#btn').click(function() {
      alert('Hello!');
    });
  }
);
```

这里将更多的逻辑转移到组件中。例如，在组件中，最好不要依赖于ID，因为相同的组件会被渲染多次。 我们可以使用 [React事件系统](React快速开始/React快速开始（六）事件处理.html)，直接在React &lt;button>元素上注册点击处理程序：

[Try it on CodePen](https://codepen.io/gaearon/pen/RVKbvW?editors=1010)

```javascript
function Button(props) {
  return <button onClick={props.onClick}>Say Hello</button>;
}

function HelloButton() {
  function handleClick() {
    alert('Hello!');
  }
  return <Button onClick={handleClick} />;
}

ReactDOM.render(
  <HelloButton />,
  document.getElementById('container')
);
```

我们可以编写很多这样的组件，并使用ReactDOM.render()将它们渲染到不同的DOM容器。 当您将更多的应用转换为React时，我们可以将它们组合成较大的组件，并将部分ReactDOM.render()调用提升到更高的层次。

### 在Backbone中嵌入React
[Backbone](http://backbonejs.org/) views通常使用HTML字符串或字符串生成模板函数来为其DOM元素创建内容。 这个过程也可以通过渲染一个React组件来代替。

下面我们来创建一个Backbone视图ParagraphView，它将覆盖Backbone的render()函数，将React &lt;Paragraph>组件呈现到由Backbone(this.el)提供的DOM元素中。 这里也是使用 [ReactDOM.render()](https://facebook.github.io/react/docs/react-dom.html#render)：

[Try it on CodePen](https://codepen.io/gaearon/pen/gWgOYL?editors=0010)

```javascript
function Paragraph(props) {
  return <p>{props.text}</p>;
}

const ParagraphView = Backbone.View.extend({
  render() {
    const text = this.model.get('text');
    ReactDOM.render(<Paragraph text={text} />, this.el);
    return this;
  },
  remove() {
    ReactDOM.unmountComponentAtNode(this.el);
    Backbone.View.prototype.remove.call(this);
  }
});
```

有一点很重要：为了在被删除时，React能注销与组件树相关联的事件处理程序和其他资源，在remove方法中我们调用ReactDOM.unmountComponentAtNode()。

当一个组件从React树中删除时，清理将自动执行，但是由于我们是手动删除整个树，所以我们必须调用该方法。


## 与Model层集成
虽然通常建议使用单向数据流，如React状态，Flux或Redux，但React组件也可以使用来自其他框架和库的模型层。
### 在React组件中使用Backbone Model
在React组件使用Backbone模型和集合的最简单方法是监听各种更改事件并手动强制更新。

负责渲染模型的组件监听“change”事件，而负责呈现集合的组件监听“add”和“remove”事件。 在这两种情况下，调用 [this.forceUpdate()](https://facebook.github.io/react/docs/react-component.html#forceupdate) 来重新渲染具有新数据的组件。

在下面的示例中，List组件呈现了一个Backbone集合。

[Try it on CodePen](https://codepen.io/gaearon/pen/GmrREm?editors=0010)

```javascript
class Item extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange() {
    this.forceUpdate();
  }

  componentDidMount() {
    this.props.model.on('change', this.handleChange);
  }

  componentWillUnmount() {
    this.props.model.off('change', this.handleChange);
  }

  render() {
    return <li>{this.props.model.get('text')}</li>;
  }
}

class List extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange() {
    this.forceUpdate();
  }

  componentDidMount() {
    this.props.collection.on('add', 'remove', this.handleChange);
  }

  componentWillUnmount() {
    this.props.collection.off('add', 'remove', this.handleChange);
  }

  render() {
    return (
      <ul>
        {this.props.collection.map(model => (
          <Item key={model.cid} model={model} />
        ))}
      </ul>
    );
  }
}
```

### 从Backbone Model获取数据
上述方法需要您的React组件了解Backbone模型和集合。 如果您以后计划迁移到另一个数据管理方案，您可能希望尽可能将这部分代码集中起来。

这里有一个解决方案：将model的属性作为普通数据无论它何时更改，并将这段逻辑放在一个单独的位置。下面是一个[高阶组件](./React高级指南（十一）高阶组件.md)，将Backbone model的所有属性提取到state，并将这个数据传递到被包装组件。

```javascript
function connectToBackboneModel(WrappedComponent) {
  return class BackboneComponent extends React.Component {
    constructor(props) {
      super(props);
      this.state = Object.assign({}, props.model.attributes);
      this.handleChange = this.handleChange.bind(this);
    }

    componentDidMount() {
      this.props.model.on('change', this.handleChange);
    }

    componentWillReceiveProps(nextProps) {
      this.setState(Object.assign({}, nextProps.model.attributes));
      if (nextProps.model !== this.props.model) {
        this.props.model.off('change', this.handleChange);
        nextProps.model.on('change', this.handleChange);
      }
    }

    componentWillUnmount() {
      this.props.model.off('change', this.handleChange);
    }

    handleChange(model) {
      this.setState(model.changedAttributes());
    }

    render() {
      const propsExceptModel = Object.assign({}, this.props);
      delete propsExceptModel.model;
      return <WrappedComponent {...propsExceptModel} {...this.state} />;
    }
  }
}
```

这样就只有高阶组件需要知道Backbone model内部属性，应用程序中的大多数组件可以与Backbone解耦。

在上面的例子中，我们将Backbone模型的attributes设置成初始状态，并订阅change事件（卸载时取消订阅），当model变化时，使用模型的当前属性更新状态。 如果prop.model本身发生变化，取消订阅旧model，并订阅新model。


为了演示如何使用，我们将React组件NameInput 与Backbone model 连接，并在每次输入更改时更新其firstName属性：

[Try it on CodePen](https://codepen.io/gaearon/pen/PmWwwa?editors=0010)

```javascript
function NameInput(props) {
  return (
    <p>
      <input value={props.firstName} onChange={props.handleChange} />
      <br />
      My name is {props.firstName}.
    </p>
  );
}

const BackboneNameInput = connectToBackboneModel(NameInput);

function Example(props) {
  function handleChange(e) {
    model.set('firstName', e.target.value);
  }

  return (
    <BackboneNameInput
      model={props.model}
      handleChange={handleChange}
    />
  );
}

const model = new Backbone.Model({ firstName: 'Frodo' });
ReactDOM.render(
  <Example model={model} />,
  document.getElementById('root')
);
```

这种技术并不限于Backbone，我们可以将React与任何模型库一起使用，通过在生命周期函数中订阅其他model库的model更新，并将model数据复制到React的本地state中。
