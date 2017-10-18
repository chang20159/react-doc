>[Higher-Order Components](https://facebook.github.io/react/docs/higher-order-components.html)

# 高阶组件
高阶组件（HOC）是React中用于重用组件逻辑的一种高级技术。 HOCs本身并不是React API的一部分，而是React组件组合的一种模式。

**具体来说，高阶组件是一个函数，接收一个组件并返回一个新组件。**

```javascript
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
就像组件将props转换成UI，高阶组件将一个组件转换成另一个组件。HOC在第三方React库中很常见，例如Redux的 [connect](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options) 和Relay的 [createContainer](https://facebook.github.io/relay/docs/api-reference-relay.html#createcontainer-static-method)。

在本文中，我们将讨论为什么高阶组件是有用的，以及如何编写自己的高阶组件。

## 使用HOC解决不同组件有相似功能的问题

>[不使用ES6的React](../React高级指南/React高级指南（六）不使用ES6的React.md#mixins) 这篇有提到这个：有时完全不同的组件可能会共享一些常见的功能。 这种情况有时被称为 [cross-cutting concerns](https://en.wikipedia.org/wiki/Cross-cutting_concern)

之前我们建议用mixins解决Cross-Cutting问题，但我们意识到mixins会带来很多问题，可以阅读更多关于为什么我们要弃用mixins，以及如何将你现有的使用到mixins的组件剥离mixins。 戳 [Mixins Considered Harmful](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html)。

组件是React中代码重用的主要单元。 但是，你会发现某些模式并不适合传统组件。

例如，现在有一个CommentList组件，通过订阅外部数据源来呈现注释列表：

```javascript
class CommentList extends React.Component {
  constructor() {
    super();
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" is some global data source
      comments: DataSource.getComments()
    };
  }

  componentDidMount() {
    // Subscribe to changes
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // Clean up listener
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // Update component state whenever the data source changes
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}

```

然后，又写了一个博客组件，也是订阅相同的外部数据源，这样就写了一份模式相同的代码：

```javascript
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}

```

CommentList与BlogPost是两个不同的组件 ： 它们在DataSource上调用不同的方法，并且它们呈现不同的输出。 但是他们的实现大部分是相同的：

- 在componentDidMount中（装载后），添加一个更改监听器到DataSource。
- 在侦听器中，只要数据源发生变化，就调用setState。
- 在componentWillUnmount中（卸载时），删除侦听器。

可以想象，在一个大型应用程序中，同样的订阅DataSource和调用setState的模式会出现很多次。 我们希望能把这个逻辑抽象出来在一个地方定义，很多组件都能分享这个逻辑，这就是高阶组件最强大之处。

我们可以编写一个函数来创建订阅DataSource的组件，如CommentList和BlogPost。这里我们将这个函数命名为withSubscription，withSubscription接收一个子组件作为参数，这个组件将订阅的数据作为props。

```javascript
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```

第一个参数是被包装的组件，第二个参数是一个函数，接收DataSource和当前props,返回我们想要的数据。

当组件CommentListWithSubscription和BlogPostWithSubscription被渲染时，CommentList和BlogPost将被传递一个prop（从DataSource获取的最新的数据）：

```javascript
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

**注意，HOC不会修改传入的组件，也不会使用继承来复制组件的行为。 相反，HOC通过将原始组件包装在容器组件中来组合。 HOC是具有零副作用的纯函数。**

被包装的组件接收容器组件的所有props，还有一个新的prop（data）。HOC并不关心怎么使用、为什么需要data;被包装组件也不需要关心data从哪里来。

因为withSubscription是一个正常的函数，所以您可以添加任意数量的参数。例如，您可能希望prop.data的名称可配置，从而进一步将HOC与包装组件隔离。 或者您可以接收一个参数来配置shouldComponentUpdate或配置数据源。 这些都是可能的，因为HOC可以完全控制组件的定义。

与组件一样，withSubscription函数与被包装组件之间的联系完全是基于props的。 这样可以方便地将一个HOC转换为不同的HOC，只要它们为被包装组件提供相同的props。 例如，更改获取数据的库。


## 不要改变原始组件，请使用组合
不要在HOC内修改组件的原型

```javascript
function logProps(InputComponent) {
  //这里的写法有问题吧？不是应该直接给componentWillReceiveProps赋一个函数么，这样会报错的
  InputComponent.prototype.componentWillReceiveProps(nextProps) {
    console.log('Current props: ', this.props);
    console.log('Next props: ', nextProps);
  }
  // 返回原始组件是为了告诉你这个组件已经被改变了
  return InputComponent;
}

// EnhancedComponent将会在接收到props时console.log
const EnhancedComponent = logProps(InputComponent);

```
这样就会有几个问题：

1. 输入组件不能重用，因为被EnhancedComponent修改了
2. 更重要的是，如果将另一个HOC应用于EnhancedComponent，componentWillReceiveProps会被改变，第一个HOC的功能会被覆盖。 ??没明白
3. 这个HOC也不适用于没有生命周期方法的函数组件。

会改变输入的HOC是一个有缺漏的抽象，使用者必须知道它的内部实现从而避免与其他的HOC产生冲突。

因而，HOC应该使用组合，将输入组件包装在容器组件中：


```javascript
function logProps(WrappedComponent) {
  return class extends React.Component {
    componentWillReceiveProps(nextProps) {
      console.log('Current props: ', this.props);
      console.log('Next props: ', nextProps);
    }
    render() {
      // Wraps the input component in a container, without mutating it. Good!
      return <WrappedComponent {...this.props} />;
    }
  }
}
```
这个HOC与上面的版本有着相同的功能，同时避免了冲突的可能性，并同时适用于类组件和函数组件。 而且因为它是一个纯函数，它可以与其他HOC，甚至与它本身组合。

HOC与容器组件模式很相似，容器组件是分离处理逻辑的一种策略。 容器负责处理诸如订阅和state这样的，并将props传递给负责诸如渲染UI之类的组件。 HOC使用容器作为其实现的一部分。 你可以认为HOC是一个参数化容器组件。



## 约定：HOC传递它本身不关心的props给被包裹组件
从HOC中返回的组件应该与被包装组件具有相似的界面，因而HOC应该传递它本身不关心的props给被包裹组件（因为被包裹组件关心啊）。大多数HOC包含一个如下所示的渲染方法：

```javascript
render() {
  //将HOC需要的但不需要传递给被包装组件的props剔出来
  const { extraProp, ...passThroughProps } = this.props;

  // 将props注入被包裹组件,通常是状态值或实例方法。
  const injectedProp = someStateOrInstanceMethod;

  // 将 props 传递给被包裹组件
  return (
    <WrappedComponent
      injectedProp={injectedProp}
      {...passThroughProps}
    />
  );
}
```
这个惯例有助于确保HOC尽可能灵活和可重用。

## 约定：将组合最大化
并不是所有的HOC都一样,有时他们只接受一个参数，即被包装的组件：

```javascript
const NavbarWithRouter = withRouter(Navbar);

```

通常，HOC会接收其他参数。 下面是Relay中的一个例子，用config对象来指定组件的数据依赖关系：

```javascript
const CommentWithRelay = Relay.createContainer(Comment, config);

```

HOCs最常见的形式是像这样的：

```javascript
// React Redux's `connect`
const ConnectedComment = connect(commentSelector, commentActions)(Comment);
```

拆分一下看的更清楚：

```javascript
// connect is a function that returns another function
const enhance = connect(commentListSelector, commentListActions);
// The returned function is an HOC, which returns a component that is connected to the Redux store
const ConnectedComment = enhance(CommentList);
```

换句话说，connect是一个高阶函数，返回一个高阶组件！

这种调用方式看起来很混乱或者说不必要，但它有一个有用的特点：HOC只有一个参数。connect函数返回的HOC具有Component => Component的特点，输出类型与其输入类型相同的函数很容易组合在一起。

```javascript
// 不是这样做...
const EnhancedComponent = connect(commentSelector)(withRouter(WrappedComponent))

// 你可以使用组合工具函数compose
// compose(f, g, h) 与 (...args) => f(g(h(...args)))相同
const enhance = compose(
  // 这些都是单参数HOC
  connect(commentSelector),
  withRouter
)
const EnhancedComponent = enhance(WrappedComponent)
```

这种特点还允许connect和其他增强型HOC用作装饰器（一个实验性JavaScript提案）

很多第三方库都会提供compose工具函数，如lodash（[lodash.flowRight](https://lodash.com/docs/#flowRight)）、[Redux](http://redux.js.org/docs/api/compose.html) 和 [Ramda](http://ramdajs.com/docs/#compose).

## 约定：为方便调试，将Display Name包含其中
由HOC创建的容器组件与任何其他组件一样会在[React Developer Tools](https://github.com/facebook/react-devtools)中显示。 为方便调试，给HOC返回的组件定义一个显示名称。

最常见的方法是包装被包装组件的显示名称。如果你的高阶组件名为`withSubscription`，并且被包装组件的显示名称为CommentList，那HOC返回组件请使用显示名称：WithSubscription（CommentList）：

```javascript
function withSubscription(WrappedComponent) {
  class WithSubscription extends React.Component {/* ... */}
  WithSubscription.displayName = `WithSubscription(${getDisplayName(WrappedComponent)})`;
  return WithSubscription;
}

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
```
## 注意
关于高阶组件有一些需要注意的地方。

### 不要在render方法中使用高阶组件
React的Diff算法（称为reconciliation）使用**组件标识**来确定是否需要更新、卸载或装载。如果从render方法中返回的组件与先前的render返回的组件相同（===），React会递归的对子组件diff，如果子组件类型不相同会被完全卸载。

我们通常不需要考虑这个，但是对于HOC来说很重要，因为这意味着我们不能将HOC应用于render方法中的组件：

```javascript
render() {
  // 每次render都会创建新的EnhancedComponent，每次的结果都是不相等的（组件标识不同）
  // EnhancedComponent1 !== EnhancedComponent2
  const EnhancedComponent = enhance(MyComponent);
  // 因此每次都会先卸载老的，再装载新的
  return <EnhancedComponent />;
}
```
这不仅会产生性能问题，重新安装组件会导致该组件及其所有子项的状态丢失。所以，我们应该在组件定义之外应用HOC，这样HOC生成的组件只能创建一次，在渲染过程中只会有这一个组件标识。

在一些比较少见的情况下，需要动态应用HOC，这时你可以在组件的生命周期方法或其构造函数中操作。
### 静态方法必须复制
有时在React组件上定义一个静态方法很有用处。 例如，Relay暴露一个静态方法getFragment用来构建GraphQL片段。

当将HOC应用于组件时，原始组件被容器组件包装。那HOC返回的新组件就没有原始组件的任何静态方法。

```javascript
// Define a static method
WrappedComponent.staticMethod = function() {/*...*/}
// Now apply an HOC
const EnhancedComponent = enhance(WrappedComponent);

// The enhanced component has no static method
typeof EnhancedComponent.staticMethod === 'undefined' // true
```

为了解决这个问题，我们可以在返回之前将静态方法复制到容器组件上：

```javascript
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  // Must know exactly which method(s) to copy :(
  Enhance.staticMethod = WrappedComponent.staticMethod;
  return Enhance;
}
```

但是，这需要我们准确地知道需要复制哪些方法。 但是我们可以使用[hoist-non-react-statics](https://github.com/mridgway/hoist-non-react-statics)来自动复制所有非React静态方法：

```javascript
import hoistNonReactStatic from 'hoist-non-react-statics';
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  hoistNonReactStatic(Enhance, WrappedComponent);
  return Enhance;
}
```


```javascript
// Instead of...
MyComponent.someFunction = someFunction;
export default MyComponent;

// ...export the method separately...
export { someFunction };

// ...and in the consuming module, import both
import MyComponent, { someFunction } from './MyComponent.js';
```

另一个可能的解决方案是将组件的静态方法与组件本身分开导出。

### 不能传递Refs
高阶组件一般会将所有的props传递给被包装的组件，但是不可能传递ref。 ref和key一样，并不是真正的prop,是由React特别处理的。 如果想要给一个元素添加ref，该元素的组件是HOC返回的组件，则ref引用的是最外层的容器组件的实例，而不是被包装组件的实例。

如果你遇到了这个问题，应该想想怎么避免使用ref，如前面介绍过的 [Refs与DOM](./React高级指南（三）Refs与DOM.md#将dom的refs暴露给父组件)，可以使用props传递ref回调。例如：

```javascript
function Field({ inputRef, ...rest }) {
  return <input ref={inputRef} {...rest} />;
}

// Wrap Field in a higher-order component
const EnhancedField = enhance(Field);

// Inside a class component's render method...
<EnhancedField
  inputRef={(inputEl) => {
    // This callback gets passed through as a regular prop
    this.inputEl = inputEl
  }}
/>

// Now you can call imperative methods
this.inputEl.focus();
```

这并不是一个完美的解决方案。 我们希望ref仍然是React本身需要关注的，而不是要求您手动处理它们。 我们正在探索解决这个问题的方法，以便使用HOC时不需要关注这些。

