
>[Conditional Rendering](https://facebook.github.io/react/docs/conditional-rendering.html)

在React中，可以选择在不同条件下渲染不同组件。

这里有两个组件：

```javascript
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

现在创建一个Greeting组件，根据用户是否登录，显示其中一个组件：

```javascript
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

ReactDOM.render(
  // Try changing to isLoggedIn={true}:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```
这个例子根据props中的isLoggedIn渲染出不同的内容

## 元素变量
可以使用变量来存储元素，并且根据条件存储元素，这样可以实现有条件地渲染。

这里有两个组件，注销和登录按钮：

```javascript
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

现在创建一个有状态组件LoginControl，它将根据当前状态呈现&lt;LoginButton />或&lt;LogoutButton />

[Try it on CodePen](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

```javascript
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;

    let button = null;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

声明变量并使用if语句,这是有条件地呈现组件的一个好方法，但有时希望使用较短的语法。 
在JSX中有几种内联条件的方法，如下所述。

## 逻辑运算符&& （内联If ）
可以在JSX中嵌入任何表达式并将其包裹在花括号中，JavaScript逻辑&&运算符就可以放在{}中。 

它可以像这样控制元素呈现：[Try it on CodePen](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

```javascript
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

在JavaScript中，true && 表达式的结果为表达式的结果，并且 false && 表达式 总是计算为false。

因此，如果条件为真，则&&后面的元素将显示。 如果是false，React会忽略并跳过它。

## 条件运算符（内联if-Else）
另一种在元素内部条件渲染的方法是使用JavaScript条件运算符  ? true : false.
下面的示例，有条件地呈现一小段文本。

```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

也可以用于更大的表达式：

```javascript
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn ? (
        <LogoutButton onClick={this.handleLogoutClick} />
      ) : (
        <LoginButton onClick={this.handleLoginClick} />
      )}
    </div>
  );
}
```

这个可以根据你认为更易读的方式选择合适的方法。 

还要记住，如果条件太复杂，可以试试提取组件

## 阻止组件渲染
在极少数情况下，可能希望组件隐藏自身，如果是这样，你可以返回一个null。
[Try it on CodePen](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

```javascript
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true}
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(prevState => ({
      showWarning: !prevState.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

ReactDOM.render(
  <Page />,
  document.getElementById('root')
);
```

**注意： 从组件的render方法返回null不会影响组件生命周期方法的触发。 
例如，componentWillUpdate和componentDidUpdate仍将被调用。**