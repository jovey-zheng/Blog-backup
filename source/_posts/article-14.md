title: 使用 React 是业务需求，而不是技术需求
date: 2015-12-15 12:23:23
categories:
  - 好文分享
tags:
  - React
  - JavaScript
---

# 前言

React 已经在开发者圈越来越流行了，并且有很多说明其技术优势的资源。然而，迁移（或是选择）一个新的框架最终归结为向所有人推销 —— 包括非开发者。这里会有一小部分工程经理或项目经理会因为它的新颖，而选择使用它来重构，更糟的是，很多团队被 JavaScript 的高生产工具搞得焦头烂额了，可悲的是向后移动项目是向前移动 web 的一部分。这篇文章并不是试图教你关于 React 的新东西，只是试着去总结以下，起点是为了向所有人说明 React 的疑惑，不仅是开发者。

**总结**：React 是一个为创建可组合的用户界面的库。同比其他类似的库如  Angular、Backbone、Knockout 和 Ember，React 的出现是为了解决业务问题而非技术的。接下来会为你解释 React 的重要性和对开发团队的益处。

# 降低风险

**稳定性** —— Facebook 花了很大的精力在 React 上（Newsfeed，Instagram，Messenger，Ads Marketplace 等），并且拥有专职的技术团队来维护这个项目。它的 dog-food 测试和投资都不是现有的任何一个项目能比的。除了 Facebook 内部的工程师，还有一大批 React 的爱好者。随着版本的更迭，目前在 github 上拥有 571 个贡献者（截止到 2015 年 12 月）。

**正在使用 React**：AirBnB，Asana，Atlassian，BBC，Chegg，CloudFlare，CNN.com，Codecademy，Coursera，Craftsy，Dailymotion，Dropbox，Expedia，Facebook，Feedly，Flipboard，HipChat，IMDb，Imgur，Instagram，Khan Academy，KISSmetrics，Mattermark，Minerva Project，Netflix，OkCupid，Rackspace，Rally Software，Ralph Lauren，Reddit，Redfin，Salesforce，Squarespace，The New York Times，Trunk Club，Twitter，Uber，University of Cincinnati，Venmo，WhatsApp，Wired，Wix，WordPress，Yahoo，Zendesk

# 开发效率

**强大的路径迁移** —— React 允许开发者可以根据自己的需求将其放到任何一个已经存在的页面上。值得注意的是，React 是需要加载一个运行时的库（React 0.14.0 的大小是 39.4 kb），因此零碎的迁移会导致页面重量的增加，直到旧版本的库被移除才会减少。

**默认情况下的性能** —— React 的使用模式，让它很难写出低性能的代码。此外，自从它减少了与 DOM 的直接交互，使得它不仅可以代替现有的一些库（Angular/Backbone/Ember），也不再需要大量像 jQuery 一样的依赖，从而减少整体部署代码大小。

**SEO** —— SEO 是从服务器发送一个已经渲染好的页面到浏览器上。React 在设计时就考虑到了 SEO，它用 Node 可以在客户端或服务端进行渲染。其他工具允许在服务端进行渲染，但需要引入一些不稳定的 hack，同时还需要大量的人员去维护。而 React 有可能简化构建工具和减少维护成本。

提高了代码的重用 —— React 在提供良好的性能同时还可以管理组件的渲染生命周期，如此一来可以显著地提高开发人员的开发效率。通过可重用组件的创造、分配和使用，使之更简单，这样开发人员就能更好地节省使用和开发通用组件的时间。就如按钮一样的低阶元素和可折叠元素一样的高阶元素。

# 提高开发效率

**从混合资源中降低复杂度** —— React 混合了 HTML 和 JavaScript，在此原则下，它们被紧紧地捆在一起，而分离它们是分离技术，这不是关注点。这个概念可以进一步扩展到 CSS，删除 CSS 开发过程中的一连串问题，包括全局命名、作用域/变量的隔离。详细请看：[Radium](http://projects.formidablelabs.com/radium/) 和 [React: CSS in js](https://speakerdeck.com/vjeux/react-css-in-js)。

**