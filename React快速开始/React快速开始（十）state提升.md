
>[Lifting State Up](https://facebook.github.io/react/docs/lifting-state-up.html)

# state提升
这篇翻译的时候啰里吧嗦，其实就是介绍了一种兄弟组件之间通信的方式。。。

通俗的解释一下：
>父组件有子组件A和子组件B，子组件A接收父组件传递的一个函数，这个函数可以更新父组件的state，子组件在用户操作时调用这个函数，这样就可以改变父组件的state了。

>那如果父组件把这个state作为props传递给它的另一个子组件B，那结果不就是 子组件A中的操作通过父组件影响了子组件B么。。。


喏，挫挫的翻译开始了。。。

有时候会有几个组件有相同state的情况，我们建议把共享的state提升到最近的共同的祖先上。 

这里，我们创建一个温度计算器来计算水在一定温度下是否沸腾。

这里有一个BoilingVerdict组件， 它接收celsius温度作为props，并打印是否可以让水沸腾：

```javascript
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```
现在，我们创建一个Calculator组件，它有一个&lt;input>可以输入温度，并将输入的值保存在this.state.temperature中，并且会根据当前输入的值渲染BoilingVerdict组件.

[Try it on CodePen](https://codepen.io/valscion/pen/VpZJRZ?editors=0010)

```javascript
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(temperature)} />
      </fieldset>
    );
  }
}
```

现在我们除了要输入摄氏温度，还要输入华氏温度，那我们再加一个输入框。我们可以从Calculator中提取一个TemperatureInput组件。 并且提供一个props属性scale，它的值可能是“c”或“f”

```javascript
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

现在Calculator是这样的,有两个输入，输入其中一个时，另一个不更新。 但我们希望能保持同步。


[Try it on CodePen](https://codepen.io/valscion/pen/GWKbao?editors=0010)

```javascript
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

首先，我们要写两个函数，摄氏度和华氏度的转换，然后返回：

```javascript
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

这两个函数都是转换数字的。 我们编写另一个函数，将字符串温度和转换器函数作为参数，并返回一个字符串，然后用它来根据一个input来计算另一个input的值。

当temperature不合法时返回空字符串，返回值四舍五入到小数点后三位：

```javascript
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

例如，tryConvert('abc',toCelsius）返回一个空字符串，而tryConvert('10.22',toFahrenheit）返回'50.396'.

现在两个TemperatureInput组件都将其输入值保存在自己的state。

但是，我们希望这两个输入是相互同步的。当我们更新摄氏温度的输入值时，华氏温度输入框也能显示转换了的华氏温度，反之亦然。

在React中，可以通过将state移动到最接近的共同祖先共享state，这叫做“提升state”。现在从TemperatureInput中删除本地state，并将其移动到Calculator中。

如果计算器拥有共享state，那两个输入中显示的当前温度数据就是这个共享的state中获得,这个共享state可以作为组件的props传递给组件。

但是props是只读的。 当temperature保存在本地状态时，TemperatureInput可以调用this.setState()来更改它。 但是，现在temperature来自parent的props，无法控制temperature。

但是可以通过接收父容器的onTemperatureChange来改变state,当TemperatureInput更新温度时，调用this.props.onTemperatureChange.


翻译不下去了，好啰嗦。。。直接看最后代码吧

[Try it on CodePen](http://codepen.io/valscion/pen/jBNjja?editors=0010)

```javascript
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}

class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```

看下编辑input时会发生什么：

1. React调用在DOM &lt;input>上onChange事件处理函数。在上面的例子中，就是TemperatureInput组件中的handleChange方法。
2. TemperatureInput组件中的handleChange方法用输入的值调用this.props.onTemperatureChange()
3. 根据编辑的输入框调用相应的函数，分别是handleCelsiusChange 和handleFahrenheitChange，更新state
4. **state改变后，通知React重新获取render方法的返回，得到最新的UI展示对象。**
5. 与原有的render返回对比后，更新必要的真实DOM重新渲染
6. 另一个输入框更新为转换后的温度

提升state需要比双向绑定写更多的代码，但有一个好处是：可以很方便的找bug。因为state都是在某一个组件中，这样定位问题的范围就可以大大缩小。

此外，还可以实现自定义逻辑拒绝或转换用户输入。（比如在onChange事件处理函数中对输入进行校验，校验不通过就不更新state,给出提示。）
