>[Installation](https://facebook.github.io/react/docs/installation.html)  


# 安装
react很灵活，可用于各种项目。 可以使用它创建新的应用程序，也可以引入到现有的代码库中。看看下面哪一种方式是你需要的。

## 如果只想试一试React

如果只想试一试React，那就使用CodePen，不需要安装任何东西，直接写React代码就能看到效果。

可以试一下 [Hello World example code](https://codepen.io/gaearon/pen/rrpgNB?editors=0010)

如果使用自己的文本编辑器，可以像下面这样写一个html文件，就可以在本地直接用浏览器打开看到效果。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello World</title>
    <script src="https://unpkg.com/react@latest/dist/react.js"></script>
    <script src="https://unpkg.com/react-dom@latest/dist/react-dom.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.15.0/babel.min.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="text/babel">

      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('root')
      );

    </script>
  </body>
</html>
```

## 创建React应用程序

看这里 &gt;&gt; [create-react-app](https://github.com/facebookincubator/create-react-app)，这是构建新的React单页应用程序的最佳方法，能够使用JavaScript最新特性

```bash
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

这样创建的应用程序是零配置的，没有使用babel和webpack

## 在已有项目中引入React

一个经典的React项目应该具备这些：

* 包管理器

  比如[Yarn](https://yarnpkg.com/zh-Hans/) 或者 [npm](https://www.npmjs.com/)，使用它可以更方便的管理第三方包，并且很方便的安装和更新包

* 打包工具

  比如[webpack](https://webpack.js.org/) 或者 [Browserify](http://browserify.org/)，它可以让你编写模块化代码并将各个模块打包在一起，以优化加载时间。

* 编译器

  如[Babel](http://babeljs.io/)， 可以将新的JavaScript特性编译成适用于旧版浏览器的JavaScript代码。

在项目中安装React

```
yarn init
yarn add react react-dom
```

```
npm install --save react react-dom
npm init
```

Yarn 和 npm 都是从[npm registry](https://www.npmjs.com/)下载包的

