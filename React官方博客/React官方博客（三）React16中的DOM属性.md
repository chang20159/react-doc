>[DOM Attributes in React 16](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html)

在之前的版本中，React会忽略未知的DOM属性。如果我们在JSX中使用了React无法识别的属性，React会将它忽略。例如:

```xml
// Your code:
<div mycustomattribute="something" />
```

在React v15中会被渲染成一个空的div: 

```xml
// React 15 output:
<div />
```

在React v16中我们做了一些修改，未知属性会保留，在React v16中的输出是：

```xml
// React 16 output:
<div mycustomattribute="something" />
```

## 我们为什么这么做？

React始终为DOM提供Javascript API,由于React组件经常有自定义的和与DOM相关的props，这就需要React像DOM API 一样使用驼峰式命名。例如：

```xml
<div tabIndex="-1" />
```

然而这种方式需要我们维护一个有效的React DOM属性的白名单:

[react/packages/react-dom/src/shared/possibleStandardNames.js](https://github.com/facebook/react/blob/master/packages/react-dom/src/shared/possibleStandardNames.js)

```xml
// ...
summary: 'summary',
tabIndex: 'tabindex'
target: 'target',
title: 'title',
// ...
```

这里有两个缺点: 

- 无法[传递自定义属性](https://github.com/facebook/react/issues/140)。这对于提供特定浏览器的非标准属性、尝试新的DOM API以及与第三方库集成非常有用。
- 这个属性列表会越来越大，但是大多数React规范属性名称已经在DOM中有效， 删除大多数白名单可以减少一点打包后包的大小。

新的方法可以解决这两个问题，在React 16中，你可以给所有HTML和SVG元素传递自定义属性，在生产版本中，React不再包含所有的属性白名单。

**请注意，对于已知属性，仍然应该使用规范的React命名：**

```xml
// Yes, please
<div tabIndex="-1" />

// Warning: Invalid DOM property `tabindex`. Did you mean `tabIndex`?
<div tabindex="-1" />
```

换句话说，在React中使用DOM组件的方式并没有改变，但是现在您有了一些新功能。

## 我可以奖数据存放在自定义属性中吗？

我们不鼓励您将数据存放在DOM属性中。 即使你必须要传递数据，`data-`可能是一个更好的方法，但在大多数情况下，数据应该保存在React组件状态或外部store中。

但是，如果需要使用非标准的或新的DOM属性，或者需要与依赖于这些属性的第三方库进行集成，那么这个新功能就很方便了。

## data和aria属性

和之前一样，React可以让你自由的传递`data-`和 `aria-`属性

```xml
<div data-foo="42" />
<button aria-label="Close" onClick={onClose} />
```

[可访问性](https://reactjs.org/docs/accessibility.html)非常重要，所以即使React 16允许传递任何属性，但在开发模式下仍然会验证`aria-`是否具有正确的名称，这和React 15是一样的。

## 迁移

在[React 15.2.0版本](https://github.com/facebook/react/releases/tag/v15.2.0)之后，我们可能会收到[关于未知属性的警告](https://reactjs.org/warnings/unknown-prop.html)。绝大多数的第三方库已经更新了它们的代码。如果您的应用没有因为使用React 15.2.0或更高版本产生警告，那这次的升级不需要您修改应用。

如果您将非DOM属性传递给了DOM组件，那使用React16 您就可以在DOM中看到这些属性了。例如：

```xml
<div myData='[Object object]' />
```
这种情况，浏览器虽然还是会忽略它们，但还是建议修复它。一个潜在的危险是，如果你传递一个实现一个自定义toString（）或valueOf（）方法的对象。 另一个可能的问题是，传统的HTML属性（如align和valign）现在将被传递给DOM，它们以前会被忽略。

为了避免这些问题，我们建议在升级到React 16前，先将React 15中的这些警告清除。

## 细节变化

我们已经做了一些其他更改使行为更加可预测，并帮助您不犯错误。 但我们不能预知这些变化是否会对您的实际项目产生影响。

**这些变化只会影响DOM组件，像&lt;div&gt;**,而不是自定义的组件。

下面是具体细节：

- 给未知属性传递string, number,和 object类型值
	- React 15: 警告并忽略
	- React 16：将数据转换成string,并传递给DOM
	- 注意：以on开头的属性不会传递，因为可能会成为潜在的安全漏洞。


```xml
<div mycustomattribute="value" />
<div mycustomattribute={42} />
<div mycustomattribute={myObject} />
```
	
- 已知属性用不规范的react名称
	- React 15: 警告并忽略
	- React 16: 发出警告，但将数据转换成string,并传递给DOM
	- 注意：对于所有支持的属性要用规范的命名方式

```
<div tabindex="-1" />
<div class="hi" />
```


- 给非布尔属性传递布尔值
	- React 15: 将布尔值转换成string,并传递给DOM
	- React 16: 警告并忽略

```
<div className={false} />
```


- 给非事件属性传递函数
  - React 15: 将函数转换成string,并传递给DOM
  - React 16: 警告并忽略

```
<div className={function() {}} />
```


 - 给属性传递Symbol类型值
	- React 15: 崩溃
	- React 16: 警告并忽略

```
<div className={Symbol('foo')} />
```
 	

- 给属性传递NaN
	- React 15: 将NaNs转换成string,并传递给DOM
	- React 16: 将NaNs转换成string,并传递给DOMwith a warning

```
<div tabIndex={0 / 0} />
```
	


在测试此版本时，我们还为所有已知属性创建了一个[自动生成的表](https://github.com/facebook/react/blob/master/fixtures/attribute-behavior/AttributeTableSnapshot.md)来查看结果。

## 试一试

可以在这里试一下这些改变 [CodePen](https://codepen.io/gaearon/pen/gxNVdP?editors=0010)

## 最后

React v15.6.2(September 25, 2017)开始，采用 MIT协议