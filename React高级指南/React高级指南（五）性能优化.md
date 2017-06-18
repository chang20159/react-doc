>[Optimizing Performance](https://facebook.github.io/react/docs/optimizing-performance.html)

# 优化性能
React内部已经最大限度地减少更新UI时DOM操作次数，不需要做太多工作来专门优化性能。React还提供了几种方法可以加快React应用程序。

## 使用生产版本构建

默认情况下，React包含许多有用的警告，这些警告在开发中非常有用，但是也会让项目更大更慢，所以部署项目到线上时应该使用生产版本。

如果你不确定你的构建过程是否设置正确，可以通过安装Chrome的[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)进行检查。 

如果在生产模式下访问React应用页面，图标背景是深色：

![](https://facebook.github.io/react/img/docs/devtools-prod.png)

如果是在开发模式下访问React应用页面，图标背景是红色（明明是橙色嘛）：


![](https://facebook.github.io/react/img/docs/devtools-dev.png)

建议在开发应用程序时使用开发模式，将应用程序部署到线上时的使用生产模式。

那么怎样告诉你的网站使用正确的版本，最有效地配置生产构建过程呢？下面介绍了几种场景下的配置

### [create-react-app](https://github.com/facebookincubator/create-react-app)

执行`npm run build`会在项目的build/目录生成生产版本代码，如果不是在生产环境，执行`npm start`

### 引用外部文件

下面引用的直接就是生产环境版本，注意只有 .min.js结尾的才是生产环境稳定版本

```xml
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>
```

### Brunch配置

安装插件uglify-js-brunch

```
//If you use npm
npm install --save-dev uglify-js-brunch
//If you use Yarn
yarn add --dev uglify-js-brunch
```

然后使用`brunch build -p`构建

### Browserify配置

安装几个插件

```
//If you use npm
npm install --save-dev bundle-collapser envify uglify-js uglifyify 
//If you use Yarn
yarn add --dev bundle-collapser envify uglify-js uglifyify
```

执行时带上这些transforms

* [envify](https://github.com/hughsk/envify)
* [uglifyify](https://github.com/hughsk/uglifyify)
* [bundle-collapser](https://github.com/substack/bundle-collapser)
* 最后结果都pipe到[uglify-js](https://github.com/mishoo/UglifyJS2) ,[read why](https://github.com/hughsk/uglifyify#motivationusage)
  

```
browserify ./index.js \
  -g [ envify --NODE_ENV production ] \
  -g uglifyify \
  -p bundle-collapser/plugin \
  | uglifyjs --compress --mangle > ./bundle.js
```

### Rollup配置

安装插件

```
//If you use npm
npm install --save-dev rollup-plugin-commonjs rollup-plugin-replace rollup-plugin-uglify
```

```
//If you use Yarn
yarn add --dev rollup-plugin-commonjs rollup-plugin-replace rollup-plugin-uglify
```

再配置文件

```
plugins: [
  // ...
  require('rollup-plugin-replace')({
    'process.env.NODE_ENV': JSON.stringify('production')
  }),
  require('rollup-plugin-commonjs')(),
  require('rollup-plugin-uglify')(),
  // ...
]
```

### webpack配置

```
if(env == 'production'){
  new webpack.DefinePlugin({
    'process.env': {
      NODE_ENV: JSON.stringify('production')
    }
  }),
  new webpack.optimize.UglifyJsPlugin()
}
```

## 使用Chrome的Performance工具分析组件
在开发模式下，我们可以使用浏览器中的性能工具来显示组件是怎样装载、更新和卸载的。 
例如：

![](https://facebook.github.io/react/img/blog/react-perf-chrome-timeline.png)

你需要在Chrome中执行下面的操作：

1. 加载页面时，在url后加上?react_perf，例如http://localhost:3000/?react_perf
2. 打开Chrome DevTools [性能选项](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/timeline-tool)，然后点击**录制**（Record）。
3. 执行您要配置的操作。 不要记录超过20秒钟，否则Chrome可能挂起。
4. 停止录制
5. React事件在 User Timing下分组

注意，数字是相对的，在生产模式下组件会有更快的呈现速度。 但这依然可以帮助你找到那些不必要更新却依然被更新的UI，以及UI更新的深度和频率。

目前，支持此功能的浏览器只有Chrome，Edge和IE，但是我们使用标准的 [User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API) ，所以我们希望有更多的浏览器可以支持。

## 避免不必要的DOM更新
React构建并维护已渲染UI的内部表示信息（JavaScript对象），包括从组件返回的React元素。 这种UI表示信息能够让React避免在不必要的情况下创建DOM节点并访问已有的节点，因为操作DOM比操作JavaScript对象更慢.有时这个React内部表示信息被称为**“虚拟DOM”**，但它与在React Native上的工作方式相同。

当组件的props或state更改时，React通过将新返回的元素与先前渲染的元素进行比较来决定是否需要更新真实DOM。 当它们不相等时，React将更新真实DOM元素。

在某些情况下，你的组件可以通过重写生命周期函数**shouldComponentUpdate**来加速渲染，这个函数在重新render之前触发。 **此函数默认返回true**，使React执行更新：

```javascript
shouldComponentUpdate(nextProps, nextState) {
	return true;
}
```

如果你知道在某些情况下，你的组件不需要更新，则可以在shouldComponentUpdate中返回false，跳过整个渲染过程，包括在此组件上的render(）和组件下方子组件的渲染。

## shouldComponentUpdate
下面是一个组件树，对于每一个节点: 

- SCU表示shouldComponentUpdate返回什么，绿色SCU表示返回true，红色SCU表示返回false
- 绿色vDOMEq表示虚拟DOM相等，红色vDOMEq表示虚拟DOM不相等
- 圆圈表示组件是否需要更新真实DOM，绿色表示不需要，红色表示需要。

![](https://facebook.github.io/react/img/docs/should-component-update.png)

由于在以C2为根节点的子树上shouldComponentUpdate返回了false，所以React没有去渲染C2，也没有在C4和C5上调用shouldComponentUpdate。

对于C1和C3，shouldComponentUpdate返回true，所以React必须下到叶子节点并检查它们是否需要更新。 对于C6， shouldComponentUpdate返回true，由于渲染的元素与原来的不等同，就需要更新真实DOM。

最后比较有趣的是C8。 React必须渲染这个组件，但由于它返回的React元素等于之前渲染的元素相同，所以不必更新DOM。

注意，React只需要对C6进行DOM更新，这是不可避免的。对于C8，React通过与之前渲染的React元素比较来避免了对DOM进行不必要的更新。对于C2的子树和C7，因为shouldComponentUpdate返回了false,所以不需要比较虚拟DOM，并且不会调用render。


## 例子
如果只有当 props.color 和 the state.count 变量改变时组件才会更新，你可以使用shouldComponentUpdate来检查：

```javascript
class CounterButton extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```
在上面的代码中，shouldComponentUpdate 仅仅比较了props.color 和 the state.count是否改变，如果它们的值没有变化，组件就不会更新。

如果您的组件变得更加复杂，可以使用类似的方式，在props和state的所有字段之间进行“浅比较”，以确定组件是否应该更新。 这种模式很普遍，React提供了一个帮助程序来实现这个逻辑 - **只需要继承自React.PureComponent**。 所以可以用下面的代码来实现同样的事情，这种方式更简单：

```javascript
class CounterButton extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```

多数情况下，您可以使用React.PureComponent而不需要自己编写通过浅比较不能检测props或state是否发生变化，就不能使用这种方式。

对于复杂的数据结构来说这是一个问题，例如，你想要一个ListOfWords组件来呈现一个逗号分隔的单词列表，其中包含一个父组件WordAdder，我们可以单击一个按钮将单词添加到列表中。 

下面的代码将无法正常工作：

```javascript
class ListOfWords extends React.PureComponent {
  render() {
    return <div>{this.props.words.join(',')}</div>;
  }
}

class WordAdder extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      words: ['marklar']
    };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // This section is bad style and causes a bug
    const words = this.state.words;
    words.push('marklar');
    this.setState({words: words});
  }

  render() {
    return (
      <div>
        <button onClick={this.handleClick} />
        <ListOfWords words={this.state.words} />
      </div>
    );
  }
}
```

这里的问题是PureComponent会对this.props.words的旧值和新值进行简单的比较（对于引用类型比较的是引用）。 由于调用handleClick方法会让word数组发生变化，所以在ListOfWords中会对比this.props.words的旧值和新值是否相等，但对比结果是相等的，即使数组中的实际值已更改（因为数组引用值没有变化）。 因此即使ListOfWords应该呈现新单词也不会更新。

## 不可变数据的作用
避免这个问题最简单的方法就是让props 或 state原有的值不变化，返回一个新的对象。
例如在上面的handleClick方法中可以使用concat重写：

```javascript
handleClick() {
  this.setState(prevState => ({
    words: prevState.words.concat(['marklar'])
  }));
}
```

ES6提供了一种数组扩展语法，使这更容易。 如果您使用的是Create React App，则此语法默认可用。

```javascript
handleClick() {
  this.setState(prevState => ({
    words: [...prevState.words, 'marklar'],
  }));
};
```

也可以以类似的方式重写可变对象。 例如，假设有一个colormap对象，我们想编写一个将colormap.right更改为“蓝色”的函数。 我们可以这样写：

```javascript
function updateColorMap(colormap) {
  colormap.right = 'blue';
}
```

为了不改变原始对象，我们可以使用Object.assign方法：

```javascript
function updateColorMap(colormap) {
  return Object.assign({}, colormap, {right: 'blue'});
}
```

现在updateColorMap返回一个新的对象，而不是改变原来的对象。 Object.assign在ES6中，并且需要一个polyfill。(这是因为很多浏览器没有实现这个方法，可以参见 [Object.assign 浏览器兼容性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign#Browser_compatibility))

JavaScript也提供了另一种扩展对象属性的方法，也是使用ES6扩展运算符，这可以返回一个新对象：

```javascript
function updateColorMap(colormap) {
  return {...colormap, right: 'blue'};
}
```

如果您使用的是Create React App，默认情况下Object.assign和对象扩展语法都是可以使用的。

## 使用不可变数据结构

[Immutable.js](https://github.com/facebook/immutable-js) 是解决这个问题的另一种方法。 它通过结构共享提供不变的、持久的集合：

- 不可变的：集合一旦创建，就不能更改。
- 持久性：可以从先前的集合和变化中创建新的集合。 创建新集合后，原始集合仍然有效。
- 结构共享：尽可能的使用与原始集合相同的结构创建新集合，将复制减少到最低限度，提高性能。


不可变性使跟踪变化变得简单，**每次更改都会得到一个新对象，所以我们只需要检查对对象的引用是否更改**。 

例如，在这个常规的JavaScript代码中：

```javascript
const x = { foo: 'bar' };
const y = x;
y.foo = 'baz';
x === y; // true
```

尽管y被改变了，由于它与x是对同一个对象的引用，所以它们的比较返回true。 你可以用immutable.js编写类似的代码：

```javascript
const SomeRecord = Immutable.Record({ foo: null });
const x = new SomeRecord({ foo: 'bar' });
const y = x.set('foo', 'baz');
x === y; // false
```

在这种情况下，由于在改变x时返回了新的引用，我们可以安全地假设x已经改变。

还有另个库可以帮助我们使用不可变数据： [seamless-immutable](https://github.com/rtfeldman/seamless-immutable) 和 [immutability-helper](https://github.com/kolodny/immutability-helper)

不可变数据结构为您提供了跟踪对象变化的便宜方法，这就是我们实现shouldComponentUpdate所需要的。  shouldComponentUpdate + 不可变数据通常可以让React应用得到不错的性能提升。



