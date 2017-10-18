
>[Rendering Elements](https://facebook.github.io/react/docs/rendering-elements.html)

# 渲染元素

元素是React应用最小构建块，每一个元素都描述了呈现在屏幕上的内容。不同于真实的DOM元素，React元素就是一个普通的对象，创建成本很低，React DOM负责更新真实DOM与React元素匹配。

关于组件、元素和实例可以看这个 [React组件、元素和实例](../React官方博客/React官方博客（二）React组件、元素和实例)

## 将元素渲染到DOM中
假设你的HTML文件中有一个&lt;div&gt;,这个节点里面的内容都由 React DOM来管理。
```html
<div id="root"></div>
```

使用React构建的应用程序通常具有单个根DOM节点，要将React元素渲染到根DOM节点中，需要将他们传递给ReactDOM.render()
```javascript
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

## 更新已渲染元素
**React元素是不可变的**，创建元素后无法更改其子项或属性,一个元素就像一个电影中的一帧：它代表了某个时间点的UI。

根据我们目前的知识，**更新UI的唯一方法是创建一个新元素，并将其传递给ReactDOM.render()**。
看下面这个显示时间的例子,每秒钟都会执行一次ReactDOM.render() 

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

**实际上，大多数React应用只会调用ReactDOM.render()一次,在接下来的章节中，我们会学习如何将这些代码封装到有状态的组件中。**

## React仅在必要时更新
尽管我们创建了一个描述整个UI树的元素，但只有内容改变的节点才被React DOM更新。因为**ReactDOM会将元素及其子元素与之前的元素进行比较，并且只更新实际改变的DOM。**




