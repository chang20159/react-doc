>[Why did we build React?](https://facebook.github.io/react/blog/2013/06/05/why-react.html)

# 为什么我们要创造React
目前已经有很多JavaScript MVC框架了，那为什么我们要创造React，为什么你
想要使用它呢？

## React不是一个MVC框架
React是一个用于构建可组合用户界面的库，React鼓励创建可重用的UI组件来显示随时间变化的数据。
## React不使用模板
传统上，Web应用程序UI是使用模板或HTML指令构建的。 您可以使用这些抽象化的模板构建UI。React构建用户界面的不同之处在于将用户界面分解成组件， 这意味着React使用一个真正的，全功能的编程语言来渲染视图，相比于模板，有以下优势：

- **JavaScript是一种灵活、强大的、并能够构建抽象的编程语言。** 这在大型应用中非常重要。
- 通过将标记与其相应的视图逻辑统一，React可以**使视图更容易扩展和维护**。
- 通过将对标记和内容的理解融入JavaScript之中，**没有手动拼接的字符串**，因此很少有引发XSS漏洞的地方。

我们还创建了[JSX]()，一种可选的语法扩展，相对于原生JavaScript，您可能更喜欢HTML的可读性。
## 草鸡简单的响应式更新
如果您的数据是动态变化的，React会让你爱不释手。

在传统的JavaScript应用程序中，您需要查看哪些数据已更改，并强制更改DOM以保持最新。 即使AngularJS通过指令和数据绑定提供声明性接口，也需要一个 [链接函数](https://code.angularjs.org/1.0.8/docs/guide/directive#reasonsbehindthecompilelinkseparation) 来手动更新DOM节点。

React则采用不一样的方法。

当组件首次初始化时，调用render方法，生成视图的轻量级UI展示。 从该UI展示中，生成一串标记，并将其注入到document中。 当数据更改时，再次调用render方法。 为了尽可能高效地执行更新，我们将前一次调用render的返回值与新的返回值对比，生成应用于DOM的最小更改集，进行有效渲染。

>从render返回的数据既不是字符串也不是DOM节点 - 它是DOM应该是什么样的轻量级描述(其实就是一个JS对象)。

我们称这个过程为reconciliation。 可以在 [jsFiddle](http://jsfiddle.net/2h6th4ju/) 查看展示reconciliation的例子。

这种重新渲染速度非常快（TodoMVC约为1ms），开发人员不需要明确指定数据绑定。 我们发现这种方法可以更容易地构建应用程序。

## HTML只是开始
React对document有自己的轻量级表示，我们可以用它来做一些很酷的事情：

- Facebook用&lt;canvas>而不是HTML来渲染动态图表
- Instagram 是一个用React 和 Backbone.Router构建的单页面应用。开发者会定期贡献使用JSX的React代码。
- 我们在web worker中构建了运行React应用的内部原型，通过Objective-C的桥接口使用React操纵原生的iOS视图层 
- 为了SEO、性能、代码共享和整体的的灵活性，您可以在 [服务端运行React程序](https://github.com/petehunt/react-server-rendering-example)
- 事件在所有的浏览器（包括IE8）中表现一致并符合标准，所有React方式绑定的事件都使用事件代理。

快去 [facebook.github.io/react](https://facebook.github.io/react/) 看看我们已经构建了的项目吧。 我们的文档旨在通过React框架构建应用程序，但如果您有兴趣， [请与我们联系](https://facebook.github.io/react/community/support.html)！

谢谢阅读！