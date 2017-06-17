
>[Lists and Keys](https://facebook.github.io/react/docs/lists-and-keys.html)

## Lists 和 Keys
我们先来看下在JavaScript中如何转换列表，下面的代码使用map()函数将number数组中的每个数值乘以2，然后返回新的数组。

控制台打印[2, 4, 6, 8, 10]

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```
在React中，元素列表的转换与这几乎相同，像下面这样（官方文档啰里啰嗦，其实一看代码就明白了是不）
[Try it on CodePen](https://codepen.io/gaearon/pen/GjPyQr?editors=0011)


```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

然后将整个listItems数组包含在一个&lt;ul>元素中，并将其渲染到DOM中：

```javascript
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

## 列表组件
通常会在一个组件中渲染列表,我们可以将前面的例子重构成接受数字数组的一个组件，并输出一个无序的元素列表。

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

在运行此代码时，会看到一个warning，提示应该为列表项提供一个key, “key”是创建元素列表时需要包含的特殊**字符串属性**。 
在下一节会讨论为什么它很重要。

[Try it on CodePen](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)

现在来给list.map()中的列表项添加一个key:

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

## Keys

**key 可以让React知道哪些列表项目已更改、添加或删除。** 应该给每一项一个固定的key。

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

选择key的最好方法是使用一个字符串来唯一标识列表项,通常会使用数据中的ID作为key：

```javascript
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

如果没有这样一个id，可以用列表的索引作为key
```javascript
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

**注意： 如果列表可以重新排序，我们不建议使用索引作为key，因为这会很慢。 可以阅读这篇[为什么需要key](https://facebook.github.io/react/docs/reconciliation.html#recursing-on-children)，给你更深入的解释**

## 提取组件with key

key只有在周围有数组的上下文中才有作用。
例如，如果提取了ListItem组件，应该将key放在在数组中的&lt;ListItem />元素上，而不应该在ListItem本身的根元素&lt;li>上。

```javascript
function ListItem(props) {
  const value = props.value;
  return (
    // Wrong! There is no need to specify the key here:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Wrong! The key should have been specified here:
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);

```

Example: key的正确用法 
[Try it on CodePen](https://codepen.io/rthor/pen/QKzJKG?editors=0010)

```javascript
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**有一个经验法则：在map()中的元素需要添加key**

## Key在兄弟元素中必须是唯一的
数组中用到的key在兄弟元素中必须是唯一的，但是不需要全局唯一，可以在两个不同的数组中用相同的key. 

[Try it on CodePen](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)

```javascript
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

**key作为React的提示属性，不会传递给组件，所以不要将key作为你想要传递给组件的属性名**

```javascript
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```
在上面的例子中，Post组件可以读取props.id，但不能读取到props.key。

## 将map()嵌入到JSX中
在上面的例子中声明了一个单独的listItems变量并将其包含在JSX中：

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

JSX允许将任何表达式嵌入到花括号中，所以我们可以内联map()的结果：

[Try it on CodePen](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)

```javascript
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

这么做有时候会让代码更清晰，但这种方式也可能被滥用。如果map()里面嵌套很多，可以考虑提取组件。