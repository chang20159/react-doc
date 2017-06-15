
>[State and Lifecycle](https://facebook.github.io/react/docs/state-and-lifecycle.html)

前面介绍过时钟的例子，通过ReactDOM.render()来改变页面渲染内容

```javascript
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}
setInterval(tick, 1000);
```
在本节，我们将学习如何让Clock组件真正可重用和可封装， 它可以设置自己的计时器并每秒更新一次
<!-- more -->
我们可以从封装时钟开始：

```javascript
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
但是这并不能满足要求，Clock设置定时器并每秒更新UI的任务应该是Clock的实现细节，不应该由外部来控制。
我们想要的是这样一个组件：Clock组件能够自己更新自己。

```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
**为了实现这一点，我们需要在Clock组件中添加“state”，state与props相似，但它是组件私有的，完全由组件控制。**          

我们之前提到过（React快速开始（四）组件和props），定义为class的组件具有一些额外功能，**组件的state就是这样：只能在定义为class 的组件中使用**

## 将Function转换为Class
可以通过下面五个步骤将时钟functional组件转换为class组件：

1. 创建一个与functional组件相同名称的ES6 class,并继承[React.Component](https://facebook.github.io/react/docs/react-component.html)
2. 添加一个名为render()的空方法
3. 将functional组件的函数主体移动到render()方法中
4. 在render()方法体中用this.props替换props
5. 删除剩下的空函数声明


functional组件:
```javascript
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

```
class组件:
```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
Clock组件现在被定义为一个class而不是一个function，这样我们就可以使用一些额外的功能，比如state和组件的生命周期钩子方法（ lifecycle hooks）

## 在class中添加state
现在把props中的date移到state中，分三个步骤：
1. 将render()方法中的this.props.date 替换为this.state.date

```javascript
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
2. 添加一个constructor来初始化this.state

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        date: new Date()
    };
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
    **注意怎么将props传递给了React.Component的构造函数的,class组件应始终调用父类 的构造函数，并传递props作为参数。**
```javascript
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
    ```
    [关于super关键字](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super)
    >在构造函数中使用时，super关键字单独出现，必须在可以使用this关键字之前使用。此关键字也可用于调用父对象上的函数。
 
3. 将date属性从 &lt;Clock /&gt;中移除

    ```javascript
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
    ```

结果是这样的
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
下一步，我们让Clock组件设置它自己的定时器，每秒钟更新自己

## 向class中添加生命周期方法
 
在有很多组件的应用程序中，在销毁组件时释放组件占用的资源非常重要。

- 当Clock第一次添加到DOM时，我们要设置一个定时器， 这在React中称为“挂载”。
- 当Clock产生的DOM被删除时，我们也想清除该计时器， 这在React中称为“卸载”。
- 当组件挂载和卸载时，我们可以在组件类上声明特殊的方法来做一些事情：

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
如上componentDidMount和componentWillUnmount，
这些方法称为 生命周期钩子函数 "lifecycle hooks"

在组件已经添加到DOM树之后，会执行componentDidMount()，在这里最适合设置一个定时器
```javascript
 componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```
注意，我们将timerID 保存在this中。

**this.props由React设置，this.state具有特殊的含义（根据用户操作改变自身状态），如果需要存储一些不用于更新页面渲染的字段，可以将它添加到类中，比如这里的timerID。**

不在render()中使用的，不应该把他放在this.state中，比如这里的timerID

在组件componentWillUnmount()生命周期钩子中拆下计时器：
```javascript
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

最后，我们实现每秒运行的tick()方法。它使用this.setState()来更新组件状态。
[Try it on CodePen](https://codepen.io/gaearon/pen/amqdNA?editors=0010)
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```


让我们快速回顾一下发生了什么以及方法调用的顺序(渲染过程)：

1）当<Clock />传递给ReactDOM.render()时，React调用Clock组件的构造函数。由于Clock需要显示当前时间，所以它new一个当前时间对象来初始化this.state。稍后会更新此状态。（constructor）

2）React调用Clock组件的render()方法,获取应该渲染的内容，更新DOM，使其与Clock的render输出一致。（render ）

3）当Clock组件插入DOM中时，React调用componentDidMount()生命周期钩子。在这里，Clock组件要求浏览器设置一个定时器每秒调用一次tick()。（componentDidMount）

4）浏览器每秒钟调用tick()方法。在这里，Clock组件通过调用setState()更新UI，显示当前时间。因为调用了setState()让React知道state有变化，并再次调用render()方法来了解屏幕上应该呈现是什么。这一次，render()方法中的this.state.date与之前不同，因而render输出将包含更新的时间，并相应地更新真实DOM。

5）如果时钟组件从DOM中删除，React会调用componentWillUnmount()生命周期钩子，让定时器停止计时。

## 正确使用state

关于setState()方法，你应该知道三件事情：

### 不要直接修改state
比如下面这样做，不会重新渲染组件
```javascript
// Wrong
this.state.comment = 'Hello';
```

应该使用setState()
```javascript
// Correct
this.setState({comment: 'Hello'});
```
**唯一可以直接给state赋值的地方是在 constructor中**

### state更新可能是异步的
在对setState()多次调用的情况下，为了提高性能，React**可能**将他们合并，最后一次性更新。
由于this.props和this.state**可能**会异步更新，所以不应该依靠它们的值计算下一个状态。

例如下面这样更新counter,页面中显示1（把官方文档例子改了一下，感觉这样更能说明问题）

```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
        counter:0
    };
  }

  componentDidMount() {
   //这里state并没有立即更新
    this.setState({
         counter: 5
      });
      //setState()接收一个对象，this.state.counter还是0
     this.setState({ 
       counter: this.state.counter + 1 
     });       
    }
    render(){
      return(
        <span>{this.state.counter}</span>
      )
    }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
下面这样，页面显示6，控制台打印：
```
state:  0
prevState:  Object {counter: 5}
setState callback1: 6
setState callback2: 6
```

```javascript
class App extends React.Component {
      constructor(props) {
        super(props);
        this.state = {counter:0};
      }

      componentDidMount() {
         this.setState({
           counter: 5
         },function(){
            console.log('setState callback1:',this.state.counter)
         });
        //setState接收一个函数，这里的prevState是上面this.setState执行完之后的state
         this.setState((prevState, props)=>{
            console.log('prevState: ',prevState)
           return {
             counter: prevState.counter + 1
           };
         },function(){
            console.log('setState callback2:',this.state.counter)
         });
         console.log('state: ',this.state.counter)
      }
    render(){
      return(
        <span>{this.state.counter}</span>
      )
    }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
先执行了console.log('state: ',this.state.counter)，说明setState()是异步更新的
setState()有两种形式
1、setState() 接收一个对象，在setState()中访问this.state可能不是最新的state
2、setState(）接收一个函数，这个函数接收最新的state作为第一个参数，更新应用后的props作为第二个参数

### state更新会被合并
当调用setState()时，React会将你提供的对象合并到当前state，例如，state可能包含几个独立变量：
```javascript
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
}
```
你可以调用setState()单独更新一个变量：

```javascript
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
}
```

**state合并是浅合并(The merging is shallow)，所以调用this.setState（{comments}）后，this.state.post依然不变，但this.state.comments会被完全替代。**

官方文档例子讲得不够明白，我再举一个例子：
```javascript
constructor(props) {
    super(props);
    this.state = {
        info: {
            value: 1,
            text: '个人',
            status: 0
        },
        array:[1,2]
    };
}

componentDidMount() {
     this.setState({
       info: {
         value: 2,
         text: '公司'
       },
       array:[2]
     },function(){
        console.log('state updated:',JSON.stringify(this.state))
     });
}
```

控制台打印结果是： 
```
{
    "info":{
        "value":2,
        "text":"公司"
    },
    "array":[2]
}
```
**所以，记住了！！state更新是浅合并！！state中的属性值会被完全替代**

## state从上至下传递
state是局部的、组件私有的，一个组件的父组件和子组件都不能知道这个组件是有状态还是无状态，它们也不需要关心这个组件是被定义为函数还是类。 除了拥有并设置它的组件之外，任何其他组件都不能访问这个state。

但是组件可以通过props将它的state传递给子组件
```html
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

在自定义组件中也可以
```html
<FormattedDate date={this.state.date} />
```

FormattedDate 组件在它的props中添加一个date字段， 在组件内并不care这个date是Clock的 state, 还是Clock的 props, 还是手动写入的
```javascript
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```
**这通常被称为“自顶向下”或“单向”数据流。**
任何state始终由某个特定组件所有，并且这个组件的state只能影响它下面的子组件。

可以将一个组件树的props想象成瀑布，每个组件的state就像一个额外的水源，在某一个点与props会合，并且也往下流动。

为了证明所有组件都是真正孤立互不影响的，我们可以创建一个App组件，呈现三个&lt;Clock&gt;：
```javascript
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
每个Clock都设置自己的定时器并独立更新。
在React应用中，不管组件是有状态还是无状态，都认为是组件自身的实现细节.
可以在有状态组件中使用无状态组件，反之亦然。



