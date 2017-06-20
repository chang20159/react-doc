>[Reconciliation](https://facebook.github.io/react/docs/reconciliation.html)

# 协调算法（Diff算法）

React提供了一个声明式的API，因此您不必担心每次更新发生了什么变化。 这让编写应用程序变得容易得多，但是在React中这是如何实现的呢？本文介绍了React中的“diff”算法，这种算法让组件更新可预测，同时对于高性能应用程序来说足够快。

## 动机
当你使用React时，可以思考一下创建一个React元素树的render()函数。在下一次 state 或 props 更新时，render()函数会返回一个不同的React元素树。React需要知道如何有效的更新UI来与当前返回的React元素树匹配一致。

这是一个需要以最小操作次数实现从一棵树转为另一棵树的算法问题，这个问题有一些通用的解决方案。然而[标准的diff的算法](http://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey_bille.pdf)复杂度为O(n<sup>3</sup>)，n是树中元素个数。

如果在React中使用这个算法，显示1000个元素需要大约10亿次比较，这无法满足性能要求。 然而React团队基于Web页面的特点做了两个简单的假设，将时间复杂度为降为O(n)：

1. 不同类型的两个元素会产生不同的树，相同类型的元素产生相似的DOM结构
2. 对于同一层次的子节点可以通过key让react知道是否添加、删除或更改

实际上，这些假设对于几乎所有的实际用例都是有效的。

## diff算法
当比较两棵树时，React首先比较两个根元素，对于不同类型的根元素，比较行为是不同的。

### 元素类型不相同
当根元素类型不同时，React会删除旧的树，重新构建新的树。例如从&lt;a>到&lt;img>, 从&lt;Article> 到 &lt;Comment>, 从&lt;Button> 到 &lt;div> ，都会重新构建。

当删除旧的树时，老的DOM节点会被销毁，执行组件实例的componentWillUnmount()方法；当构建一个新的树时，新的DOM节点会插入到DOM树中，会执行组件实例的 componentWillMount()和componentDidMount()；任何与旧的树相关的state都会丢失，根组件下的任何子组件都会被销毁。

例如：

```xml
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```
这会将老的 <Counter />销毁，并创建一个新的 <Counter />。



### 相同类型的DOM元素
当比较两个相同类型的DOM元素时，React会查看两者的属性，保留相同的底层DOM节点，并且只更新已更改的属性。例如：

```xml
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```
通过比较这两个元素，React知道只修改底层DOM节点上的className。

更新样式时，React也知道只更新更改的属性。 例如：

```xml
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />
```

这里React只会修改color，而不会修改fontWeight。

对DOM节点diff处理后，React再对子节点进行递归diff。

### 相同类型的组件元素
当组件更新时，实例保持不变，从而在渲染过程中保存状态。 React更新底层组件实例的props以匹配新元素，并调用底层实例上的componentWillReceiveProps()和componentWillUpdate()。

接下来，调用render()方法，并且对上一个返回结果和新结果进行递归diff。
### 在Children上递归
默认情况下，当对DOM节点的子节点进行递归时，React会同时遍历两个子列表.
例如，当在列表的最后添加一个元素时，这两个树之间的转换很简单：

```xml
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>

```

React会匹配第一个li &lt;li>first&lt;/li> 和第二个li &lt;li>first&lt;/li>，然后插入第三个li。

但是如果在列表最前面插入一项，性能就会变差。例如：

```xml
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```
react会替换掉每一项， Connecticut替换Duke，Duke替换Villanova，然后插入Villanova，而不知道保留Duke和Villanova。这样效率会很低。
### Keys
为了解决这个问题，React提供一个属性key。 当children有key时，React使用该key将原始树中的列表与新的树中的列表相匹配。 例如，

```xml
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```
现在React知道key为2014的元素是一个新的元素，key为2015和2016的元素只需要移动一下。

通常指定一个key并不困难,您要显示的元素可能已经具有唯一的ID，因此key可以来自你的数据：

```xml
<li key={item.id}>{item.name}</li>
```

key必须在兄弟节点中是唯一的，但在全局环境下不要求唯一。

你也可以将数组中的项目索引作为key。 如果项目永远不会重新排序，可以使用这种方法。如果会重新排序，执行会很慢，建议不要使用索引。
## 权衡
React的这种diff算法基于两个假设，如果这个假设不能满足，性能就会受到影响。

- 该算法不会尝试匹配不同组件类型的子树。如果两个组件输出很相似但类型不同，应该让它们类型相同。
- key应该是稳定的、可预测的、唯一的。不稳定的键（如Math.random()生成的）将导致许多组件实例和DOM节点被不必要地重新创建，这会导致子组件的性能下降和状态丢失。

## 总结
这篇主要是讲react更新的细节：diff算法。 这篇翻译的不太好，不能完全概括diff算法。可以参考这篇[深入浅出React（四）：虚拟DOM Diff算法解析](http://www.infoq.com/cn/articles/react-dom-diff?from=timeline)，讲的很详细。