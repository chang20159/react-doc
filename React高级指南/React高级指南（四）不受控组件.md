# 不受控组件
大多数情况下，我们建议使用受控组件来实现表单。 在[受控组件](../React快速开始/React快速开始（九）表单.md)中，表单数据由React组件来处理。 替代方案是不受控制的组件，其中表单数据由DOM本身处理。还有一种替代方案就是使用不受控组件，不受控组件的表单数据由DOM本身处理。

编写一个不受控制的组件，就不是为每个可变状态提供事件处理程序了，而是使用ref从DOM获取表单值。

例如，下面的不受控组件，接收一个单一的name:

[Try it on CodePen](https://codepen.io/gaearon/pen/WooRWa?editors=0010)

```javascript
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={(input) => this.input = input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```
由于不受控组件将DOM的真实数据保存在DOM中，在使用不受控组件时，集成React和非React代码更为容易，也可以扫尾减少一点代码量。如果没有这种需求，还是建议使用受控组件，由React来控制form表单。

如果仍然不清楚在某种场景下应该使用受控组件还是不受控组件，这篇关于受控与不受控输入的文章 [controlled-vs-uncontrolled-inputs-react](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/) 可能对你有帮助。

## 指定默认值defaultValue
在React渲染生命周期中，form元素上的value属性会覆盖DOM中的值。 使用不受控制的组件时，通常希望React指定初始值，但后续更新后就不再显示初始值。 要处理这种情况，可以指定一个defaultValue属性而不是value属性。

```javascript
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={(input) => this.input = input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```
同样，&lt;input type =“checkbox”>和&lt;input type =“radio”>支持defaultChecked，而&lt;select>和&lt;textarea>支持defaultValue。