JSX其实就是React.createElement（component，props，... children）函数的语法糖。 

JSX代码：

```xml
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

编译后：

```javascript
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```

如果没有children，也可以用自闭合标签

```xml
<div className="sidebar" />
```

编译后：

```javascript
React.createElement(
  'div',
  {className: 'sidebar'},
  null
)
```

如果想知道JSX怎么转换成JavaScript，可以在[Babel在线编译器](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-0&targets=&browsers=&builtIns=false&debug=false&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyA)上试试

## 指定React元素类型
JSX标签的首字母就决定了React元素的类型。

首字母大写表示JSX标记指的是React组件。 这些标签会被编译成对命名变量的直接引用，所以如果使用JSX &lt;Foo />表达式，Foo必须在可访问范围内。

### React必须在可访问范围内
JSX编译后是对React.createElement的调用，所以React库必须始终在JSX代码的访问范围内。

例如，尽管React和CustomButton在JavaScript代码中没有直接用到，但这两个导入都是必需的：

```javascript
import React from 'react';
import CustomButton from './CustomButton';

function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```
如果是用&lt;script>加载的React，React可以在全局范围内使用，不需要再引入。

### 对JSX类型使用点符号
还可以使用JSX中的点表示法引用React组件。 如果从一个模块导出很多React组件，例如，MyComponents.DatePicker是一个组件，可以直接在JSX中引用：

```javascript
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

## 用户定义的组件必须大写开头
当元素类型以小写字母开头时，它表示一个内置的组件，如&lt;div>或&lt;span>，并将一个字符串'div'或'span'传递给React.createElement。 

以大写字母开头的类型，如&lt;Foo />编译为React.createElement（Foo），并与在JavaScript文件中定义或导入的组件对应。

建议用大写字母命名组件，如果你的组件以小写字母开头，在JSX中使用之前请将其分配给大写的变量。

例如，这段代码将无法按预期运行：

```javascript
import React from 'react';

// Wrong! This is a component and should have been capitalized:
function hello(props) {
  // Correct! This use of <div> is legitimate because div is a valid HTML tag:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // Wrong! React会认为<hello />是一个HTML标签，因为不是大写开头:
  return <hello toWhat="World" />;
}
```

下面是正确写法：


```javascript
import React from 'react';

// Correct! This is a component and should be capitalized:
function Hello(props) {
  // Correct! This use of <div> is legitimate because div is a valid HTML tag:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // Correct! React knows <Hello /> is a component because it's capitalized.
  return <Hello toWhat="World" />;
}
```

### 在运行时确定类型

不能使用通用表达式作为React元素类型。 如果想要使用通用表达式来表示元素类型，请先将其分配给大写的变量。 当你要根据props渲染不同的组件时，会出现这种情况：

```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Wrong! JSX type can't be an expression.
  return <components[props.storyType] story={props.story} />;
}

```

应该先把计算类型的表达式分配给一个大写开头的变量：

```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

## JSX中的Props

在JSX中有几种不同的方式来指定props

### JavaScript表达式
可以将一个JavaScript表达式传给props，用{}括起来
例如：

```javascript
<MyComponent foo={1 + 2 + 3 + 4} />
```

对于MyComponent，props.foo的值为表达式1 + 2 + 3 + 4的计算结果:10

**在JavaScript中,if语句和for循环不是表达式，所以不能直接在JSX中使用。** 
可以把逻辑提取出来，结果赋给一个变量，再把这个变量给props 

例如：

```javascript
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```

可以在前面的章节中，了解 [条件渲染]() 和 [循环]() 的更多信息
### 字符串
也可以给props传一个字符串，下面两种方式是等同的

```jsx
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />

```

如果要传递没有转义的字符串，下面这两种JSX表达式是等价的

```jsx
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

注意：如果这样写：

```jsx
<MyComponent message={"&lt;3"} />
```

会被编译成：

```javascript
React.createElement(MyComponent, { message: "&lt;3" });
```
 {}中的字符串不会被转义


### props默认为“真”
如果没有给一个props传值，它默认为true。 下面这两个JSX表达式是等价的：

```jsx
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

ES6中，对象 {foo}是 {foo: foo}的简写，而不是 {foo: true}的简写，所以一般不建议使用默认为“真”这种写法（上面第一种写法），容易混淆。但是它确实符合HTML的行为。

参见 [ES6 属性定义](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Object_initializer#属性定义)
### spread运算符
现在有一个对象，如果想把对象中的属性分别传给props，而不是把整个对象传给props,可以使用...扩展运算符， 这两个组件是等效的：


```javascript
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

在构建通用容器时，扩展属性可能很有用。 但是也可能会传递很多组件不需要的props，所以建议谨慎使用此语法。

## JSX中的Children
在有开始标签和结束标签的JSX表达式中，开始和结束标签之间的内容作为一个特殊的props，通过props.children传递。 有几种不同的方法来传递children：

### 字符串
在开标签和闭标签之间放入字符串文字，访问props.children就会是那个字符串

例如：
```javascript
<MyComponent>Hello world!</MyComponent>
```

这样组件中的props.children将只是字符串“Hello world！”，HTML会被转义，所以可以像写HTML一样写JSX 

```javascript
<div>This is valid HTML &amp; JSX at the same time.</div>

```

babel会编译为：


```javascript
React.createElement(
  "div",
  null,
  "This is valid HTML & JSX at the same time."
);
```

JSX会在行的开始和结尾处移除空格，也会删除空行， 删除与标签相邻的新行; **在字符串文字中间的换行会被压缩成一个空格。**下面这些渲染结果都已一样的：

```html
<div>Hello World</div>

<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```

最后都会编译成：

```javascript
React.createElement(
  "div",
  null,
  "Hello World"
);
```

### JSX元素
可以将多个JSX元素作为Children，这对显示嵌套组件很有用：

```jsx
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>

```

也可以像相面这样混合：

```html
<div>
  Here is a list:
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
</div>
```

一个React组件不能返回多个React元素，但单个JSX表达式可以有多个子元素，因此，如果希望一个组件呈现多个元素，可以将它包含在一个div中。

### JavaScript表达式
也可以将任何JavaScript表达式作为Children，并包含在{}中。 

例如，这些表达式是等价的：

```jsx
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```

这样可以呈现任意长度的JSX表达式的列表。 

例如：

```javascript
function Item(props) {
  return <li>{props.message}</li>;
}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}
```
JavaScript表达式可以与其他类型的Children混合使用。 

这通常用于代替字符串模板：

```javascript
function Hello(props) {
  return <div>Hello {props.addressee}!</div>;
}
```


### 函数

插入JSX中的JavaScript表达式计算结果通常是字符串，React元素或内容列表。 但是，props.children可以像其他prop一样传递任何种类的数据，而不仅仅是React知道如何呈现的种类。 例如，如果你有自定义组件，可以将一个回调函数作为props.children：

```javascript
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}

```

这个例子只是为了说明传递给自定义组件的children可以是任何类型，只要组件在呈现之前可以将它转换成React能够理解的东西，上面这种做法并不常见，只是为了拓展你对JSX的理解。

上面的例子可以改写成这样：

```javascript
function Repeat(props) {
  return <div>{props.children}</div>;
}

function ListOfTenThings() {
	let items = [];
	for (let i = 0; i < 10; i++) {
	items.push(<div key={i}>This is item {i} in the list</div>);
	}
   return (
	    <Repeat >
	    	{ items}
	    </Repeat>
  );
}

```
### Booleans, Null, and Undefined会被忽略
false，null，undefined和true是有效的children，但它们不会渲染。 
下面这些JSX表达式呈现相同：

```html
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

这对于React元素的条件渲染很有用，下面的例子，如果showHeader为true，仅呈现<Header />：

```html
<div>
  {showHeader && <Header />}
  <Content />
</div>
```

要注意的是： 有一些假值，比如数字0，仍然会被React渲染。

例如，下面的代码将不会按您预期的那样运行，当props.messages为空数组时会渲染出 ‘0’：

```javascript
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
```

要解决这个问题，请确保&&之前的表达式始终为布尔值
```javascript
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```

所以，如果想要一个呈现false，true，null或undefined这样的值中，必须先将其 [转换为字符串](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String#将其他值转换成字符串)：

```html
<div>
  My JavaScript variable is {String(myVariable)}.
</div>
```