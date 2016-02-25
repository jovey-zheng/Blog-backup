title: React 入门实践
date: 2016-01-26 09:43:59
categories:
  - React
tags:
  - React
  - 入门
  - 实践
---

> 在写这篇文章之前，我已经接触 React 有大半年了。在初步学习 React 之后就正式应用到项目中，当时就想把自己的一些想法写出来分享一下，无奈不太会写文章，再则时间不是很充裕，所以也就搁下了。
本篇文章比较基础，没有深入的分析，大神们轻看。废话就不多说了，那么让我们来进入正题。

# 简介
首先想要介绍的是 React，看到这篇文章的朋友想必都有一些关于 React 的了解了，但对于刚接触的新人而言，在这就要简要地介绍一下了。然后就是关于使用 React 构建一个简单单页应用（下文用 SPA 代替，Single Page Application）的一些介绍和讲解。

## 关于 React
React 起源于 Facebook 的内部项目，因为该公司对市场上所有 JavaScript MVC 框架，都不满意，就决定自己写一套，用来架设Instagram 的网站。做出来以后，发现这套东西很好用，就在2013年5月开源了。（[更多相关介绍请看这](http://baike.baidu.com/item/react/18077599#viewPageContent)）

特点：
* **仅仅只是 UI**
* **虚拟 DOM**：最大限度减少与 DOM 的交互（类似于使用 jQuery 操作 DOM）
* **单向数据流**：很大程度减少了重复代码的使用

组件化：
* 可组合（Composeable）：一个组件易于和其它组件一起使用，或者嵌套在另一个组件内部。如果一个组件内部创建了另一个组件，那么说父组件拥有（own）它创建的子组件，通过这个特性，一个复杂的UI可以拆分成多个简单的UI组件
* 可重用（Reusable）：每个组件都是具有独立功能的，它可以被使用在多个UI场景
* 可维护（Maintainable）：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护

<!-- more -->

生命周期：
* Mounting：已插入真实 DOM
* Updating：正在被重新渲染
* Unmounting：已移出真实 DOM

React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。
* componentWillMount()
* componentDidMount()
* componentWillUpdate(object nextProps, object nextState)
* componentDidUpdate(object prevProps, object prevState)
* componentWillUnmount()

此外，React 还提供两种特殊状态的处理函数。
* componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
* shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用

# 正题
那么进入正题，花了点时间去写一个简单的 SPA，也算是一个比较完整 React 骨架，但不包括测试（测试的教程可以看[这个](http://www.jianshu.com/p/6c74c96148c9)），相关源码可以查看 [react-start-kit](https://github.com/jovey-zheng/react-start-kit)。

接下来看看我们这个项目的构建需要用到些什么：
 * react
 * redux
 * webpack
 * react-router
 * ant design
 * babel
 ...

还有一些没有列举出来，具体可以看仓库源码的 [`package.json`](https://github.com/jovey-zheng/react-start-kit/blob/master/package.json)。其中的详细介绍会在文尾列出一些我所看过的文章或是官方介绍。

## 配置项
### Webpack
说到 React 项目的构建就不得不提 Webpack 这个神器。构建工具有很多，例如 Grunt，Gulp，Brunch 等，相比这些构建工具，Webpack 感觉就是和 React 不谋而合，尤其是 [react-hot-loader](https://github.com/gaearon/react-hot-loader) 这样的神器（热加载），让 Webpack 成为最主流的 React 构建工具。

关于 Webpack 的特性以及介绍这里就不赘述了，我们可以从下图看出 Webpack 的作用：
![](http://cdn4.infoqstatic.com/statics_s2_20160120-0059u2/resource/articles/react-and-webpack/zh/resources/0602005.jpg)

接着我们从项目代码中来看 Webpack。
```
entry: {
  app: [__dirname + '/src/index'],
},
output: {
  path: __dirname + '/_dist',
  filename: '[name]_[hash].js',
}
```
这部分主要是指定入口和出口文件。`entry` 作为项目的入口文件；`output` 作为文件编译后的出口，其中 `path` 代表输出的路径，`filename` 代表文件名称，而 `[name]_[hash]` 保证了浏览器不会存在缓存（即修改文件后效果不生效）。

```
module: {
  loaders: [{
    test: /\.js$/,
    loaders: ['babel'],
    exclude: /node_modules/,
  }, {
    test: /\.css$/,
    loaders: ['style', 'css'],
    include: /components/,
  }, {
    test: /\.(jpe?g|png|gif|svg|ico)/i,
    loader: 'file',
  }, {
    test: /\.(ttf|eot|svg|woff|woff2)/,
    loader: 'file',
  }, {
    test: /\.(pdf)/,
    loader: 'file',
  }, {
    test: /\.(swf|xap)/,
    loader: 'file',
  }],
}
```
而这部分会帮助我们去处理不同类型的文件，其中 `test` 就是文件的后缀，`loaders` 是“转译器”，`include` 是指定文件的目录，`exclude` 是排除某个目录。我们可以看出，所有的 `.js` 文件都会通过 babel 去转译，也就是我们在项目中使用 ES6+ 语法会通过 babel 转译成浏览器可以识别的 ES5 代码。

最后配置好的 config 是这样的：
```
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    app: [__dirname + '/src/index'],
  },
  output: {
    path: __dirname + '/_dist',
    filename: '[name]_[hash].js',
  },
  resolve: {
    root: [
      __dirname + '/src',
      __dirname + '/node_modules',
      __dirname,
    ],
    extensions: ['', '.js'],
  },
  module: {
    loaders: [{
      test: /\.js$/,
      loaders: ['babel'],
      exclude: /node_modules/,
    }, {
      test: /\.css$/,
      loaders: ['style', 'css'],
      include: /components/,
    }, {
      test: /\.(jpe?g|png|gif|svg|ico)/i,
      loader: 'file',
    }, {
      test: /\.(ttf|eot|svg|woff|woff2)/,
      loader: 'file',
    }, {
      test: /\.(pdf)/,
      loader: 'file',
    }, {
      test: /\.(swf|xap)/,
      loader: 'file',
    }],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: __dirname + '/src/index.html',
      favicon: __dirname + '/src/favicon.ico',
      inject: false,
    }),
  ],
};
```
### Express 服务器启动
Node.js web 应用开发框架 Express 作为项目的 web 服务器，有 Node.js 开发经验的同学应该挺熟悉的了，这里也不多做赘述。

最终的启动代码是这样的：
```
var express = require('express');
var webpack = require('webpack');
var webpackConfig = require('./webpack.development');

var app = express();
var compiler = webpack(webpackConfig);

app.use(require('webpack-dev-middleware')(compiler, {
  stats: {
    colors: true,
  },
}));

app.use(require('webpack-hot-middleware')(compiler)); //热加载

app.listen(process.env.PORT, function(err) { //在没有端口的情况下，会自动给出一个随机端口
  if (err) {
    console.log(err);
  }
});
```
为了方便我们的访问，项目使用了 `minihost` 进行启动，方便快捷。值得一提的是，使用 `h -- npm start` 命令启动时，访问的是项目文件夹的名称作为链接，例如项目叫 `myproject`，那么此时可以访问 `myproject.t.t`。

## Redux
对于复杂的 SPA，状态（state）管理非常重要。state 可能包括：服务端的响应数据、本地对响应数据的缓存、本地创建的数据（比如，表单数据）以及一些 UI 的状态信息（比如，路由、选中的 tab、是否显示下拉列表、页码控制等等）。如果 state 变化不可预测，就会难于调试（state 不易重现，很难复现一些 bug）和不易于扩展（比如，优化更新渲染、服务端渲染、路由切换时获取数据等等）。

> state 为单一对象，使得 Redux 只需要维护一棵状态树，服务端很容易初始化状态，易于服务器渲染。state 只能通过 dispatch(action) 来触发更新，更新逻辑由 reducer 来执行。

在使用 Redux 后，state 就变得很容易维护，而且数据流非常清晰，容易解决遇到的 BUG。

我们可以看下图来简要地理解 Redux：
![](http://pic3.mojiax.com/mdimg/2016225/o_1acbp1n1r1qq9107d1enudoa15dtc.png)

我们可以在项目中看到的结构是：
```
├─store
├─actions
├─reducers
├─constants
├─helpers
├─components
├─app.js
├─favicon.ico
├─index.html
├─index.js
└─routes.js
```
最终我们的 store 是：
```
import {createStore, applyMiddleware, combineReducers, compose} from 'redux';
import thunk from 'redux-thunk';
import {reduxReactRouter} from 'redux-router';
import createHistory from 'history/lib/createHashHistory';

import routes from '../routes';
import * as reducers from '../reducers';

let middlewares = [thunk];

if (process.env.NODE_ENV === 'development') { //在开发环境下可以看到 state 的 log
  const logger = require('redux-logger');
  middlewares = [...middlewares, logger];
}

const finalCreateStore = compose( //组合多个函数
  applyMiddleware(...middlewares),
  reduxReactRouter({routes, createHistory}),
)(createStore); //创建 store 来管理所有的 state

export default function configureStore(initialState) {
  const reducer = combineReducers(reducers);  //把一个由多个不同 reducer 函数作为 value 的 object，合并成一个最终的 reducer 函数
  const store = finalCreateStore(reducer, initialState);

  if (process.env.NODE_ENV === 'development' && module.hot) { //开发环境下的热加载
    module.hot.accept('../reducers', () => {
      const nextReducers = require('../reducers');
      const nextReducer = combineReducers(nextReducers);
      store.replaceReducer(nextReducer);
    });
  }

  return store;
}
```
获取 state 需要在组件中调用 `connect` 函数，可以自行定义需要获取的 state。（这用于区分展示型和容器型组件）
```
...
@connect(
  state => ({
    data: state.data
  })
)
export default class ComponentOne extends Component {
  ...
}
```
**注意**：`connect` 必须紧跟 component 的定义，不然会报错。

## Router
为项目添加路由系统，使用了 react-router 来管理路由。在开发项目的时候，比较推荐的做法是使用路由去跳转页面，并且创建 store 的同时我们就把 router 加入其中，然后我们根据路由的变化去更新视图。

我们可以看看路由的源码：
```
import React from 'react';
import Route from 'react-router/lib/Route'; //import {Route} from 'react-router';
import Base from 'components/base/Base';
import Home from 'components/home/Home';

export default (
  <Route component={Base}>
    <Route path="/" component={Home} />
    <Route path="/home" component={Home} />
  </Route>
);
```
`path` 是跳转路径，`component` 是与路径相匹配的组件。

## Ant Design
由蚂蚁金服技术部出品的一个 UI 设计语言，也是项目中所用到的 UI 组件库。

特性：
* Designed as Ant Design，提炼和服务企业级中后台产品的交互语言和视觉风格
* [React Component](http://react-component.github.io/badgeboard/) 上精心封装的高质量 UI 库
* 基于 npm + webpack + babel 的工作流，支持 ES2015

选择理由：
* 有很好的技术支持
* 简洁的样式
* 基本涵盖常用组件
...

## 简单的 Component
组件作为 React 渲染的一个基本组成，我们通常把它们分为两类，**容器型**和**展示型**。相较于**容器型**，**展示型**是通过**容器型**传递 props 来获取数据，而**容器型**可以直接从 store 中获取，处理并传递给下级组件。

在实际应用中会发现，定义一个容器型组件负责处理数据，然后分发给下级展示型组件，当需要更新数据时，那么容器型组件发生变化会引起下级展示型组件的变化，这样就对我们业务上造成了一定的困扰（在不需要更新的部分组件上也发生了更新）。因此，我们选择在需要获取数据的组件中使用 `connect`，这样则会方便很多（感觉有些违反规则）。

在项目中我们会这么定义组件：
```
import React, {Component} from 'react';
import {connect} from 'react-redux';
import Presentational from 'components/common/Presentational';

@connect(
  state => ({
    data: state.data
  })
)
export default class Container extends Component {

  render() {
    const {data} = this.props;

    return (
      <Presentational data={data} />
    )
  }
}
```
上面是可以从 store 获取数据的组件，并嵌套另一个组件，将数据传递给它。
```
import React, {Component, PropTypes} from 'react';

export default class Presentational extends Component {

  static propTypes = {
    data: PropTypes.string,
  }

  render() {
    const {data} = this.props;

    return (
      <div>
        {data}
      </div>
    )
  }
}
```
获取上一个组件传递过来的数据，并展示出来。

# 总结
这是一篇科普文（哈哈~囧），并没有深入去分析各项技术的具体内容，希望能帮助刚入手 React 的新手们。实践项目的源码可以在 [react-start-kit](https://github.com/jovey-zheng/react-start-kit) 看到，你可以下载这个项目进行自己的一些探索和开发。还在努力探索中，文中有措辞不当或是疏漏，欢迎提出意见和建议。

# 参考
[react 官网](http://facebook.github.io/react/)
[Babel 官网](https://babeljs.io/)
[redux 介绍](http://segmentfault.com/a/1190000003503338?_ea=323420)
[redux 中文文档](http://camsong.github.io/redux-in-chinese/)
[Ant design 官网](http://ant.design/)
[React 入门实例教程](http://www.ruanyifeng.com/blog/2015/03/react.html)
[react-router 中文文档](http://react-guide.github.io/react-router-cn/)
[Webpack 傻瓜式指南（一）](http://zhuanlan.zhihu.com/FrontendMagazine/20367175)
[CSS Modules 详解及 React 中实践](http://zhuanlan.zhihu.com/purerender/20495964)
[一看就懂的 ReactJs 入门教程（精华版）](http://www.cocoachina.com/webapp/20150721/12692.html)
[深入浅出React（二）：React开发神器Webpack](http://www.infoq.com/cn/articles/react-and-webpack?utm_source=tuicool)
