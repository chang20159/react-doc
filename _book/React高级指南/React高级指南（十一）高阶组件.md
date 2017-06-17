## 简介

本脚手架集成了webpack+react+redux+cortex+es6+less+postcss+babel+immutable,支持Hot Module Replacement (HMR)，对于逻辑交互比较复杂的项目，且不考虑ie9以下浏览器的情况下，使用react可降低开发的复杂度。
本项目还集成了：

* `eagle-ui` 用于pc端项目的react组件库框架
* `phoenix-ui` 这是一款基于react开发的一套wap端组件框架。需要依赖phoenix-style库
* `phoenix-styles` 单纯的样式库，配合文档给出的html结构使用，目前使用文档在项目中的example里
* `eg-tools` 提供一些较为基础的类库，如双向绑定、store可视视图调试工具、fetch、loadingbar等
* `classnames` 组装className


## 目录结构

.    
├── LICENSE    
├── README.md    
├── cortex.json    
├── dist    //最终生成的文件    
├── f2eci.json  //Peon CI 配置    
├── gulpfile.js    
├── package.json    
├── src    
│   ├── actions   //来描述“发生了什么,多用于跟server端数据交互，通过dispatch方法更新 state    
│   │   ├── index.es6   //示例    
│   │   └── msg.es6     //示例    
│   ├── components    //组件、页面组件    
│   │   ├── wap    
│   │   └── web   //示例    
│   │       ├── content    
│   │       │   ├── Content.jsx    
│   │       │   └── Content.less    
│   │       ├── header    
│   │       │   ├── Header.jsx    
│   │       │   ├── Header.less    
│   │       │   └── logo.png
│   │       ├── msg    
│   │       │   ├── Msg.jsx    
│   │       │   ├── Msg.less    
│   │       │   └── SuccessDialog.jsx    
│   │       └── readme    
│   │           ├── AccessInfo.jsx    
│   │           ├── ConfigInfo.jsx    
│   │           ├── CreatePcProjectInfo.jsx    
│   │           ├── CreateWapProjectInfo.jsx    
│   │           ├── DirInfo.jsx    
│   │           ├── Info.jsx    
│   │           ├── MattersInfo.jsx    
│   │           ├── Readme.jsx    
│   │           └── Readme.less    
│   ├── config    //配置文件    
│   │   ├── alias.json      //webpack alias配置，用于映射复杂路径文件，将复杂路径文件用一个别名的方式表示，参考webpack api    
│   │   ├── base.config.js    //项目基本信息配置，包括入口文件（root）、调试默认打开的页面等    
│   │   ├── externals.json    //webpack externals配置，类库的外部依赖，参考webpack api    
│   │   └── vendor.json     //webpack vendor配置，参考webpack api    
│   ├── constants   //常量    
│   │   └── action-type.es6    
│   ├── containers    //页面主容器，用于将碎片化的组件在页面中组装起来    
│   │   ├── wap    
│   │   └── web   //示例    
│   │       ├── Index.jsx    
│   │       ├── Msg.jsx    
│   │       └── Parcel.jsx    
│   ├── entries   //单页面打包方式入口    
│   │   ├── index.jsx    
│   │   └── msg.jsx    
│   ├── html    //静态页和mock数据    
│   │   ├── index.html    
│   │   ├── mocks //用于模拟本地环境的测试数据    
│   │   │   ├── msg    
│   │   │   │   └── save.json    
│   │   │   └── search.json    
│   │   └── msg.html    
│   ├── index.jsx   //单页面路由方式入口，SAP    
│   ├── reducers    //store    
│   │   ├── home.es6    
│   │   ├── index.es6    
│   │   └── msg.es6    
│   └── utils   //工具类库    
│       └── readme.md    
├── webpack-dev.config.js    
└── webpack.config.js    

## 命名规范

* `containers` 和 `components` 里的文件应该已Class方式存放，命名规范为首字母大写，之后驼峰式。
* `entries` 和 `html` 文件名应保持一致。
* `action`、`index`、`entries`、`reducers`、`constants` 里的文件命名规则为单词之间应以（-）连接，所有单词应保持小写方式。

## 入口

项目中如果想使用路由的方式进行页面间跳转，通过在config/base.config.js里修改root选项操作，默认是src/index.js或entries目录，通过路由跳转的方式多用于pc端的单页面应用(SPA)；			

如果想每个页面引入页面自己的脚本文件，修改root=entries，entries目录下对应每个页面自己的脚本。注意页面名和entries下文件名保持一致，多用于开发hybrid应用。

## Command

```

	#打包	
	npm run build	
	
	#本地演示dev
	npm start
```

## 发布方式

点评内部通过dianpingoa中的ci方式发布，ci类型请选择 ** peon_static **

注：关于peon_static 发布方式请至：[http://wiki.sankuai.com/pages/viewpage.action?pageId=531468248 ](http://wiki.sankuai.com/pages/viewpage.action?pageId=531468248)   查看文档。

## 页面预览

访问html页面 h5.dianping.com/app/appName/path/to/file.html		

访问其余静态资源 www.dpfile.com/app/appName/path/to/file.min.md5.ext		

appName 指的是package.json中的name字段 			

beta环境对应的域名分别为 h5.51ping.com 和 s1.51ping.com			

## 回滚

目前此发布方式的回滚功能还在开发，为了保险起见，每次通过分支进行合并开发发布。

## 引用

点评内部通过cortex方式在页面中引用dist下的文件，其他同学需根据自己的实际情况而定。

- cortex引入方式

```
	<cortex:css resource="/app/jquery-project-template/test.css" decorate="true"></cortex:css>
	<cortex:js resource="/app/jquery-project-template/jquery.js" decorate="true"></cortex:js>
	<cortex:js resource="/app/jquery-project-template/test.js" decorate="true"></cortex:js>
```

## 本地调试

### 前端资源调试

- 执行npm(cnpm) install
- 执行cortex install
- 执行npm run dev 启动本地环境，预览页面

### 后端调试

在java项目.ftl中引入通过 npm run dev启动好的链接文件

```
	<script>
		window.ENV={
			actionName:'actionName' //对应action中定义的名字
		};
	</script>
	<script src="http://127.0.0.1:3005/dist/bundle.js"></script>
```
或通过配置判断环境引入不同环境的文件

```

	#if Request['isLocal'] >
    <script src="http://127.0.0.1:3005/dist/bundle.js"></script>
    <#else>
        <cortex:css resource="/app/jquery-project-template/test.css" decorate="true"></cortex:css>
		<cortex:js resource="/app/jquery-project-template/jquery.js" decorate="true"></cortex:js>
		<cortex:js resource="/app/jquery-project-template/test.js" decorate="true"></cortex:js>
        <@cortex.jsFramework/>
        <@cortex.facadesPlaceHolder/>
    </#if>
```

** 如果有sso，请将sso配置文件禁用掉，或者手动配置sso信息 **


