
>[Introducing JSX](https://facebook.github.io/react/docs/introducing-jsx.html)

JSX，全称 JavaScript XML ，一种类XML语言，它是JavaScript的语法扩展。

没有使用JSX
```javascript
var element = React.createElement(
  "h1",
  null,
  "Hello, world!"
);
```

使用JSX
```javascript
const element = <h1>Hello, world!</h1>;
```
可以看出使用JSX可以让代码可读性更高、语义更清晰、更易维护。JSX类似于模板引擎，但功能更强大
<!-- more -->
## 语法
**1、可以在JSX中嵌入任何JavaScript表达式，方法是将其包装在花括号中。**

  ```javascript
  const element = (<h1>Hello, {formatName(user)}!</h1>);
  ```
**2、将JSX分割成多行，可读性更好。**    
这不是必需的，但在这样做的时候，建议把它放在括号中，以避免自动分号插入的陷阱。

  ```javascript
  const element = (
    <h1>
      Hello, {formatName(user)}!
    </h1>
  );
  ```
**3、JSX也是一个表达式，编译后，JSX表达式就是常规的JavaScript对象。**        
这意味着可以在if语句和for循环中使用JSX、将其分配给变量、接受它作为参数、并从函数返回它

  ```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
  ```
**4、用JSX指定属性**
  引号和花括号不能同时使用
   **注意: 在JSX中，属性使用驼峰式命名，例如class变为className,font-size变为fontSize**
  ```javascript
  //用引号指定字符串作为属性
  const element = <div tabIndex="0"></div>;
  //使用花括号将JavaScript表达式嵌入到属性中
  const element = <img src={user.avatarUrl}></img>;

  ```
  
 
**5、用JSX指定children**    
  如果标签为空，则可以使用/>关闭
  
  ```javascript
  const element = <img src={user.avatarUrl} />;

  ```
  JSX标签也可以有children       
  
  ```javascript
  const element = (
    <div>
      <h1>Hello!</h1>
      <h2>Good to see you here.</h2>
    </div>
)
  ```
**6、JSX可以防止脚本注入攻击，在JSX中嵌入用户输入是安全的**
默认情况下，React DOM会在渲染之前转义嵌入在JSX中的任何值，确保不会注入任何未明确写入应用程序的内容。 例如：
```html
<div>{'First &middot; Second'}</div>
```
花括号中的内容并不会展示为 First · Second        
在呈现之前，所有内容都将转换为字符串。 这有助于防止XSS（跨站点脚本）攻击。

  ```javascript
  const title = response.potentiallyMaliciousInput;
  // 这样是安全的
  const element = <h1>{title}</h1>;
  ```
**7、JSX的对象表示**    
Babel将JSX编译成React.createElement（）调用。  
这两个例子是一样的：

  ```javascript
  const element = (
    <h1 className="greeting">
      Hello, world!
    </h1>
  );
  ReactDOM.render(element, document.getElementById("root"));
  ```
编译后：
  ```javascript
  const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!'
  );
  ReactDOM.render(element, document.getElementById("root"));
  ```
React.createElement（）会执行一些检查帮助你编写无错误代码，
它会创建一个基本类似于如下所示的对象：
  
  ```javascript
  // 注意：这个结构被简化了
  const element = {
    type: 'h1',
    props: {
      className: 'greeting',
      children: 'Hello, world'
    }
  };
  ```
**这些对象称为“React元素”**, React读取这些对象，并使用它们构造DOM呈现页面最新状态。

**另外，使用ReactDOM.render()，React可以嵌入到使用其他JavaScript UI库的应用程序中。**

## JSX使用经验
### 使用事件
JSX采用驼峰写法来描述事件名称，例如onChange、onClick
```html
<li className='topic' onClick={::this.clickHandler}></li>
```
### 使用样式
样式属性名用-连接的都采用驼峰写法，例如font-size : fontSize,background-color:backgroundColor

- 静态样式
  ```html
<span className='triangle' style={{display:'inline-block'}}></span>
  ```
- 动态样式

  ```javascript
let spanStyle = {
  color: '#ff6633',
  fontSize: '14px'
};
<span style={{display:(spanStyle?'inline-block':'none'),...spanStyle}}></span>

  ```

### 使用表达式
{}里面都是表达式，那就可以这样写

```javascript
render(){
  return(
      <div>
          {
            this.state.show ? (
                <span>噢噢噢噢</span>
            ) : null
          }
      </div> 
  )
}
```

### 使用注释

```javascript
render(){
  return(
      <div>
        <span>噢噢噢噢</span>
        {
            //这里是注释
        }
      </div> 
  )
}
```
