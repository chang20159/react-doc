>[ReactDOMServer](https://facebook.github.io/react/docs/react-dom-server.html)

# ReactDOMServer
- 如果从&lt;script>标签加载React，这些在ReactDOMServer上的顶级API可在全局中使用
- 如果您使用ES6，```import ReactDOMServer from 'react-dom/server'```
- 如果你使用ES5，```var ReactDOMServer = require('react-dom/server')```

## 总览
ReactDOMServer对象允许您在服务端渲染组件。

- ```renderToString()```
- ```renderToStaticMarkup()```

## 参考
### renderToString()
```javascript
ReactDOMServer.renderToString(element)
```
输入React元素，将返回一个HTML字符串，这只能在服务端使用。 您可以使用此方法在服务器上生成HTML，并在初次请求时发送，以加快页面加载速度，并允许搜索引擎抓取您的页面以支持SEO。

如果您在已经具有此服务器渲染标记的节点上调用ReactDOM.render()，React将保留它并仅添加事件处理程序，从而使您具有非常优秀的首次加载体验。

### renderToStaticMarkup()
```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

与renderToString类似，但它不会创建额外的DOM属性，如React内部使用的data-reactid。 如果要使用React作为简单的静态页面生成器，没有这些额外属性是很有用的，因为可以节省大量字节。

关于服务端渲染，这里有个例子：

```javascript
import {renderToString} from 'react-dom/server'
import React from 'react'
import KoaRouter from 'koa-router'
import Hello from '../../react/server'
app.get('/hello', (ctx, next) => {
  ctx.body = `
    <html>
    <head>
      <title>title</title>
      <script src="/socket.io/socket.io.js"></script>
    </head>
    <body>
      <div id="react-target">${renderToString(<Hello />)}</div>
      {/* <div id="react-target">${renderToStaticMarkup(<Hello />)}</div> */}
      <script src="/main.js"></script>
    </body>
    </html>
  `
})
```