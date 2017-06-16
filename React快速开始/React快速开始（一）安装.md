
>[Installation](https://facebook.github.io/react/docs/installation.html)
>[Optimizing Performance](https://facebook.github.io/react/docs/optimizing-performance.html)

打算把React官方文档翻译一遍，有可能也会加入一点自己的理解。翻译出来就不用每次都去看一坨英文了，长痛不如短痛，希望能坚持下去啦。~



## 安装
react很灵活，可用于各种项目。 可以使用它创建新的应用程序，也可以引入到现有的代码库中。看看下面哪一种方式是你需要的。

### 如果只想试一试React
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

### 创建React应用程序
看这里 >> [create-react-app](https://github.com/facebookincubator/create-react-app)，这是构建新的React单页应用程序的最佳方法，能够使用JavaScript最新特性

```bash
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```
这样创建的应用程序是零配置的，没有使用babel和webpack

### 在已有项目中引入React
一个经典的React项目应该具备这些：

- 包管理器
 
    比如[Yarn](https://yarnpkg.com/zh-Hans/) 或者 [npm](https://www.npmjs.com/)，使用它可以更方便的管理第三方包，并且很方便的安装和更新包
- 打包工具 

    比如[webpack](https://webpack.js.org/) 或者 [Browserify](http://browserify.org/)，它可以让你编写模块化代码并将各个模块打包在一起，以优化加载时间。
- 编译器

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

## 开发和生产版本
默认情况下，React包含许多有用的警告，这些警告在开发中非常有用。
但是这会让项目更大更慢，所以部署项目到线上时应该使用开发版本。

那么怎样告诉你的网站使用正确的版本，最有效地配置生产构建过程呢？下面介绍了几种项目配置

### [create-react-app](https://github.com/facebookincubator/create-react-app)

执行`npm run build`会在项目的build/目录生成生产版本代码，如果不是在生产环境，执行`npm start`

### 引用外部文件
下面引用的直接就是生产环境版本，注意只有 .min.js结尾的才是生产环境稳定版本

```html
<script src="https://unpkg.com/react@15/dist/react.min.js"></script>
<script src="https://unpkg.com/react-dom@15/dist/react-dom.min.js"></script>
```
### Brunch配置

安装插件uglify-js-brunch 
    
```
//If you use npm
npm install --save-dev uglify-js-brunch
//If you use Yarn
yarn add --dev uglify-js-brunch
```

然后使用`brunch build -p`构建
    
### Browserify配置
安装几个插件
    
```
//If you use npm
npm install --save-dev bundle-collapser envify uglify-js uglifyify 
//If you use Yarn
yarn add --dev bundle-collapser envify uglify-js uglifyify 
```
执行时带上这些transforms
    
        * [envify](https://github.com/hughsk/envify)
        * [uglifyify](https://github.com/hughsk/uglifyify)
        * [bundle-collapser](https://github.com/substack/bundle-collapser)
        * 最后结果都pipe到uglify-js 
例如:
    
```
    browserify ./index.js \
      -g [ envify --NODE_ENV production ] \
      -g uglifyify \
      -p bundle-collapser/plugin \
      | uglifyjs --compress --mangle > ./bundle.js
```

### Rollup配置
安装插件
    
```
//If you use npm
npm install --save-dev rollup-plugin-commonjs rollup-plugin-replace rollup-plugin-uglify 
```

```
//If you use Yarn
yarn add --dev rollup-plugin-commonjs rollup-plugin-replace rollup-plugin-uglify
```
    再配置文件
    
```
plugins: [
  // ...
  require('rollup-plugin-replace')({
    'process.env.NODE_ENV': JSON.stringify('production')
  }),
  require('rollup-plugin-commonjs')(),
  require('rollup-plugin-uglify')(),
  // ...
]
```
### webpack配置

例如,  
    `npm run build`会根据生产版本配置文件webpack.config.js构建,  
    `npm run dev`会根据开发配置文件webpack-dev.config.js构建   
    在生产版本配置文件webpack.config.js中配置如下插件，需要生产版本就执行`npm run build`，平时开发就执行`npm run dev`
    
```
new webpack.DefinePlugin({
  'process.env': {
    NODE_ENV: JSON.stringify('production')
  }
}),
new webpack.optimize.UglifyJsPlugin()
```

