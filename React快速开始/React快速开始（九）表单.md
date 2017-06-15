
>[Forms](https://facebook.github.io/react/docs/forms.html)

form元素在React中与在普通html中的表现有些不同，表单元素本身有一些内部状态。 
例如，这个HTML中的表单接受一个单一的name：

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

当用户提交表单时，默认会跳转到新页面，在React中，这也是可以的。
但是在大多数情况下，用JavaScript函数来访问和处理用户输入表单的数据更方便，实现这一点的标准方法是使用一种称为“受控组件”的技术。
<!-- more -->

## 受控组件
在HTML中，像&lt;input>，&lt;textarea>和&lt;select>这些表单元素通常都有自己的状态，并根据用户输入进行更新。 在React中，可变状态通常保存在组件的state中，并且只能使用setState()进行更新。

我们可以结合两者，将React的state作为表单数据的唯一来源，呈现表单的React组件可以控制用户输入时表单的表现。这样一个由React来控制数据的表单元素成为‘受控组件’。

[Try it on CodePen](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

这个表单元素上设置了value属性，显示的值始终为this.state.value，使React的state成为事实上的的数据源。每一次用户输入都会触发执行handleChange方法来更新显示的值。

在受控组件中，每个可变状态都具有关联的处理函数，这样我们可以直接修改或验证用户输入。
 例如，我们要把用户输入的name转换为全部用大写字母，可以把handleChange写成：

```javascript
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

## textarea 标签
在HTML中，在&lt;textarea>元素中写文本内容：

```html
<textarea>
  Hello there, this is some text in a text area
</textarea>
```

**在React中，&lt;textarea>使用value属性。** 这样，使用&lt;textarea>的表单与使用单行输入input的表单非常类似：

```javascript
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
**注意：this.state.value必须在构造函数中初始化**

## select标签
在HTML中，&lt;select>用于创建一个下拉列表。 例如，

```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

注意，Coconut选项因为有‘selected’属性，被默认选中了。
在React中不是使用这个‘selected’属性，而是使用&lt;select>标签上的value属性。在受控组件中这样更方便，因为只需要在一个位置更新。 
例如：[Try it on CodePen](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

```javascript
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

总之，这样做能够让&lt;input >，&lt;textarea>和&lt;select>都很相似 ：它们都接受一个value属性，我们可以通过这个value来控制表单呈现。

## 处理多个输入
当需要处理多个受控输入元素时，可以给每个元素添加一个name属性，这样可以只用写一个处理函数，在这个函数里根据event.target.name的值来做不同的操作。例如：

[Try it on CodePen](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

```javascript
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

注意，这里使用了[ES6计算属性名语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Object_initializer#计算的属性名)来更新name对应的state属性

```javascript
this.setState({
  [name]: value
});
```
这与下面ES5代码等同：

```javascript
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```
另外：因为setState()会自动将setState()的输入合并（浅合并）到当前状态，所以我们只需要给setState()部分属性。

## 受控组件的替代方案
使用受控组件有时会很繁琐，因为需要为每个可变状态编写事件处理程序，并通过React组件管理所有的输入状态。如果老的代码需要转为React或将React应用程序与非React库集成时，这可能特别麻烦。 这种情况下，可能需要检查不受控制的组件(??没明白啥意思)，这是实现表单输入的替代方案。