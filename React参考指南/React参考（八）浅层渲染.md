>[Shallow Renderer](https://facebook.github.io/react/docs/shallow-renderer.html)

# 浅层渲染
引入：

```javascript
import ShallowRenderer from 'react-test-renderer/shallow'; // ES6
var ShallowRenderer = require('react-test-renderer/shallow'); // ES5 with npm
```
## 概括
在为React编写单元测试程序时，使用浅层渲染可能会有所帮助。 浅层渲染不会对子组件渲染，不必担心未实例化或呈现的子组件的行为。 这也不需要DOM。

例如，我们有这样一个组件：

```javascript
function MyComponent() {
  return (
    <div>
      <span className="heading">Title</span>
      <Subcomponent foo="bar" />
    </div>
  );
}

```
然后你可以做出断定：

```javascript
import ShallowRenderer from 'react-test-renderer/shallow';

// in your test:
const renderer = new ShallowRenderer();
renderer.render(<MyComponent />);
const result = renderer.getRenderOutput();

expect(result.type).toBe('div');
expect(result.props.children).toEqual([
  <span className="heading">Title</span>,
  <Subcomponent foo="bar" />
]);
```
浅测试目前有一些限制，即不支持refs。

## 参考

### shallowRenderer.render()
shallowRenderer.render()类似于ReactDOM.render()，但它不需要DOM，只能渲染一个层级，这样可以将组件与它的子项隔离测试。
### shallowRenderer.getRenderOutput()
在调用shallowRenderer.render()之后，可以使用shallowRenderer.getRenderOutput()来获取浅层渲染的输出。

然后，您可以对输出断言。