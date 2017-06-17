>[Refs and the DOM](https://facebook.github.io/react/docs/refs-and-the-dom.html)

# Refs 和 DOM
在典型的React数据流中，props是父组件与它的children交互的唯一方法。 要修改child，需要用新的props重新渲染。 但是，在有些情况下需要在典型数据流之外强制修改child。 

要修改的child可以是React组件的一个实例，也可以是一个DOM元素。 对于这两种情况，React提供了一个窗口。

## 何时使用Refs

Refs有一些比较适合的使用场景：

- 管理焦点，文本选择或媒体播放
- 触发强制性动画
- 与第三方DOM库集成

有些可以声明式完成的事情，就要避免使用refs。

例如，不要在Dialog组件上暴露open()和close()方法，然后在父组件中使用this.refs.dialogRef.open()来控制Dialog，而是将一个isOpen属性传递给它，通过改变this.props. isOpen来控制Dialog的打开和关闭。

## 不要过度使用Refs
你可能更倾向于使用Refs来控制事情的发生，如果是这样的话，建议多花一点时间研究一下state应该在组件树中的那个位置。

可以在 [State提升](快速开始/state提升.md) 这篇看下示例

## 给DOM元素添加Ref
React提供一个可以访问任何组件的特殊属性ref，ref属性接受一个回调函数，并且在组件被装载或卸载之后立即执行回调。

**当在HTML元素上使用ref属性时，ref回调函数接收底层的DOM元素作为参数。 **
例如，下面的代码使用引用回调来存储对DOM节点的引用：

```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.focus = this.focus.bind(this);
  }

  focus() {
    // Explicitly focus the text input using the raw DOM API
    this.textInput.focus();
  }

  render() {
    //ref的回调函数将DOM元素input的引用存放在一个实例属性中（如，这里的this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={(input) => { this.textInput = input; }} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focus}/>
      </div>
    );
  }
}
```
当CustomTextInput组件装载时，React会调用ref的回调，并将DOM元素作为参数；  
当CustomTextInput组件卸载时，也会调用ref的回调，但，是将null作为回调函数的参数

ref属性的回调函数中给类设置属性来引用DOM元素，是访问DOM元素的常见方法。 首选的方法就是像上面的例子一样在ref回调中设置属性。这里还有一个更简短的写法：

```javascript
ref = {input => this.textInput = input}
```

## 给class组件添加Ref
当在自定义的类组件上使用ref属性时，ref回调接收一个已装载的组件的实例作为参数。

例如，如果我们想模拟在CustomTextInput装载后立即被点击的效果：

```javascript
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focus();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

**注意，只有当CustomTextInput组件被声明为类组件的时候，才起作用：**

```javascript
class CustomTextInput extends React.Component {
  // ...
}

```

## Refs和函数组件
你可能不会在声明为函数的组件上使用ref属性，因为函数组件没有实例：

```javascript
function MyFunctionalComponent() {
  return <input />;
}

class Parent extends React.Component {
  render() {
    // This will *not* work!
    return (
      <MyFunctionalComponent
        ref={(input) => { this.textInput = input; }} />
    );
  }
}

```

>可以在回调中把input打印出来看一下，输出是null。

如果确实需要ref属性，可以把函数组件转换成类组件，就像在需要生命周期方法或状态时转换步骤一样。[Converting a Function to a Class](https://facebook.github.io/react/docs/state-and-lifecycle.html#converting-a-function-to-a-class)

不过我们可以在功能组件内部使用ref，只要是在DOM元素或者类组件上就行:

```javascript
function CustomTextInput(props) {
  // textInput must be declared here so the ref callback can refer to it
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={(input) => { textInput = input; }} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );  
}
```

## 将DOM的Refs暴露给父组件
在极少数情况下，我们**希望在父组件中访问子组件的DOM节点**。通常不建议这样做，因为会破坏组件的封装性，但偶尔也可以用于触发焦点或测量子DOM节点的大小或位置。

如果只是想获取组件实例的话，给[子组件添加一个ref](#给class组件添加Ref)并不是一个好方法，而且对函数组件也不起作用。


这种情况下，建议给子组件添加一个props属性，将子组件中的DOM节点的ref回调作为子组件的props传递给子组件，子组件再把这个ref回调函数给它的DOM节点的ref属性，如下示例：

```javascript
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```
子组件CustomTextInput有一个props属性inputRef，值是匿名函数el => this.inputElement = el，组件CustomTextInput将这个props.inputRef传给了它的DOM节点input的ref，这样CustomTextInput装载时就会调用匿名函数el => this.inputElement = el，并将input节点的引用传递给Parent组件的this.inputElement。

这样 在父组件中就能访问到子组件内部的组件或DOM节点了，这个子组件（比如这里的CustomTextInput）可以声明为类，也可以声明为函数。

注意，这里prop属性取名为inputRef并没有特树含义，就是一个普通的prop，只是用Ref结尾语义会更好一些。

>其实这告诉了我们一个访问函数组件的方法，上面的例子，我们可以把ref={props.inputRef}挪到CustomTextInput最顶层的节点div上。

通过给函数组件添加inputRef这样的prop，可以绕过 [只能给DOM元素和类组件添加ref属性](#Refs和函数组件)的限制。

这种将实际想要引用的节点的ref回调函数从父组件通过prop传递的方式很有用处，这种方式不仅父组件可以访问到子组件中的DOM节点，祖先组件也可以，只要将这个inputRef从祖先组件（想要访问这个DOM节点的组件）从传递下去就好了。

看下面这个例子：

```javascript
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

function Parent(props) {
  return (
    <div>
      My input: <CustomTextInput inputRef={props.inputRef} />
    </div>
  );
}


class Grandparent extends React.Component {
  render() {
    return (
      <Parent
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```
在这里 ref回调函数是在Grandparent中指定的，并作为一个prop(inputRef)传递给CustomTextInput，CustomTextInput又把它传递给了它的DOM节点&lt;input>。最后，Grandparent中的this.inputElement就会指向CustomTextInputDOM节点&lt;input>

## 旧版API：String Refs
如果你使用过老版本的React，应该知道在React老版本中ref属性是一个字符串，比如"textInput"，并通过this.refs.textInput来访问这个节点。我们建议不要再这样使用，因为字符串refs有[一些问题](https://github.com/facebook/react/pull/8333#issuecomment-271648615)，在以后的版本中可能会被废弃。如果你现在正在使用 this.refs.textInput，我们建议你替换为callback函数。

关于字符串refs的[一些问题](https://github.com/facebook/react/pull/8333#issuecomment-271648615)

1. React需要跟踪当前渲染组件，这样会让React有点慢
2. 当我们想使用回调来渲染时，例如 &lt;DataGrid renderRow={this.renderRow} />
ref这个对节点的引用就会放置在DataGrid上。

	举个例子：

	```javascript
	class MyComponent extends Component {
	  renderRow = (index) => {
	  //这样给ref一个字符串的话，ref只能在DataTable访问到，在MyComponent中获取不到
	    return <input ref={'input-' + index} />;
	
	    // 这样做是可以的，所以还是要用callback ref
	    return <input ref={input => this['input-' + index] = input} />;
	  }
	 
	  render() {
	    return <DataTable data={this.props.data}
	     		renderRow={this.renderRow} />
	  }
	}
	```
3. 使用string refs没有可组合性，但是callback ref可以

   举个例子：[issues 8734](https://github.com/facebook/react/issues/8734)
   
   ```xml
   <TransitionGroup ref={(ref) => this._refs = ref}>
    	<div>Hello</div>
	</TransitionGroup>
   ```
   
   可以通过 this._refs.refs['.$.0']来获取&lt;div>Hello&lt;/div>
   
   >这个例子是从react issues找的，自己写了个组件组合的例子，通过this._refs拿到的是空对象 --todo

## 注意事项
如果ref的callback是一个内联函数，在**更新**的时候会被调用两次，第一次传入的参数是null,第二次传入的是DOM元素或组件实例。这是因为每次render都会创建一个函数实例，所以React需要清除旧的引用并设置新的引用。

你可以通过将ref回调定义为类的方法来避免这种情况，但请注意，在大多数情况下，调用两次并没有关系。

>我写了个例子试了一下，前面文章说ref回调会在装载和卸载时立即调用，发现原来组件更新时也会调用，我理解应该是每次执行render都会调用，不知道这样理解有没有问题。

看下面这个例子，在关键地方都console.log了

```javascript
class MyComponent extends React.Component{
  render(){
    return <input onChange={this.props.onChange}/>;
  }
}
  

class Parent extends React.Component {
  
  constructor(){
    super()
    this.state = {
        value: ''
    }
  }

  componentDidMount(){
    console.log('componentDidMount.......')
  }

  changeHandler(e){
      this.setState({
          value: e.target.value
      })
  }

  render() {
    console.log('render.......')
    return (
      <MyComponent
        ref={(input) => {  
          console.log('ref callback.....',input)
          this.textInput = input; 
        }}
        onChange={this.changeHandler.bind(this)} />
    );
  }
}

ReactDOM.render(
  <Parent/>,
  document.getElementById('root')
);
```
在组件装载时，打印结果：

```
render.......
ref callback..... MyFunctionalComponent {props: Object,...}
componentDidMount.......
```

在输入框输入时会调用setState()触发更新，打印结果是：

```
render.......
ref callback..... null
ref callback..... MyFunctionalComponent {props: Object,...}
```
说明在组件更新时确实调用了两次

## 翻译总结

 - 这里可以总结一下类组件比函数组件多哪些功能了： state、life cycle hooks、ref
 - 类组件ref callback参数是组件实例， DOM元素ref callback参数是DOM
 - callback ref  vs  string ref ，string ref将会被废弃