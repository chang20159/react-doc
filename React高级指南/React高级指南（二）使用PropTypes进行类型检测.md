>[Typechecking With PropTypes](https://facebook.github.io/react/docs/typechecking-with-proptypes.html)

**注意：React.PropTypes不再适用于React v15.5，请改用库[prop-types](https://www.npmjs.com/package/prop-types)。**

# 类型检测
当应用程序体量越来越大，可以通过类型检测捕获大量错误。对于某些应用程序，可以使用像[Flow](https://flow.org/)或者[TypeScript](https://www.typescriptlang.org/)这样的JavaScript扩展来对整个应用程序进行类型检查。

但即使不使用它们，React也有一些内置的**类型检查**功能。 要对一个组件的props进行类型检查，可以给组件添加一个propTypes属性：

```javascript
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

从PropTypes可以导出很多验证器，用于检测接收的数据是否有效。 在上面这个例子中，使用PropTypes.string检测。 props.name的值无效时（这里是非string类型），**JavaScript控制台中会显示警告**。 由于考虑到性能，仅在开发模式下才会检查propTypes。

## PropTypes

以下是PropTypes提供的不同验证器的示例：

```javascript
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 可以声明一个prop是一个特定的JS原始类型。 默认情况下，这些都是可选的。
  //比如props上不要求有optionalArray，但如果有，就应该是PropTypes.array类型
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // optionalNode可以是任何可以被渲染的类型，如数字、字符串、元素或者包含这些类型的数组（片段）
  optionalNode: PropTypes.node,

  // optionalElement 应该是一个React元素
  optionalElement: PropTypes.element,
  
  //也可以使用JS的instanceof操作符声明一个prop是一个类的实例，
  optionalMessage: PropTypes.instanceOf(Message),

  // 可以将prop限定在一组特定的值内
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可以是多种类型中的一种
    optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 限定为一个特定类型的数组，如数字数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 限定对象属性值是某种特性类型
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // An object taking on a particular shape  --todo
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  //可以在上面那些情况后面链式加上isRequired，表示这个prop属性必须提供，如果没有会给出warning
  requiredFunc: PropTypes.func.isRequired,

  // requiredAny可以是任意类型，但必须提供
  requiredAny: PropTypes.any.isRequired,

  // 还可以自定义验证器。 如果验证失败，它应该返回一个Error对象。 
  //不要`console.warn`或throw，因为这在`oneOfType`里不起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 还可以自定义`arrayOf` and `objectOf`的逻辑，默认是数组或对象中的每一项必须是某种类型值。
  // 如果验证失败，应该返回一个Error对象。 
  // 验证器将被数组或对象中的每个key调用,就是每一项都参与验证逻辑
  // 验证器的前两个参数是数组或对象本身和当前项的key
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

## 要求组件只有一个child
使用PropTypes.element可以指定只有一个child可以作为props.children传递给组件。

```javascript
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // This must be exactly one element or it will warn.
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};

```

## 指定默认props
通过给组件指定默认的props值，这需要给组件添加defaultProps属性

```javascript
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

// Renders "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);

```

defaultProps用于确保当父组件未指定这个prop.name时，this.props.name也有值。 
propTypes类型检查在defaultProps解析之后执行，因此defaultProps也会参与类型检测。

## 翻译总结

- 这篇主要介绍组件上的两个特殊属性 Greeting.propTypes 和 Greeting.defaultProps
- 解析顺序是 先Greeting.defaultProps，后Greeting.propTypes，所以如果有类型检测，Greeting.defaultProps 也会参与检测
