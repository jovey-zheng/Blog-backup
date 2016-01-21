title: React 测试驱动教程
date: 2016-01-21 18:19:35
categories:
  - 好文分享
tags:
  - 译文
  - React
  - Tutorial
  - Testing
  - JavaScript
---

测试是开发周期中的一个重要组成部分。没有测试的代码被称为：遗留代码。对于我而言，第一次学习 React 和 JavaScript 的时候，感到很有压力。如果你也是刚开始学习 JS/React，并加入他们的社区，那么也可能会有相同的感觉。想到的会是：

 * 我应该用哪一个构建工具？
 * 哪一个测试框架比较好？
 * 我应该学习哪种流模式？
 * 我需要用到流吗？

为了解决这些烦恼，我决定写这篇文章。经过几个小时的博客文章阅读，查阅 JS 开发者的源码，还有参加 Florida 的 JSConf，终于让我找到了自己的测试“槽”。开始让我觉得没有经过测试的 React 程序代码是如此的不标准和凌乱。我想活在一个没有这种感觉的世界，但后来想想，这是不对的。

本教程所有的代码都可以在我的 [github 仓库](https://github.com/SpencerCDixon/react-testing-starter-kit)中找到。

让我们开始吧！

## 设置 Webpack
本教程不是一个教如何使用 webpack，所以我不会详细说，但重要的是要了解基本的东西。
Webpack 就像 Rails 中的 Assets Pipeline 一样。在基础层面上而言，在运行 react 应用时，
会在处理你的代码和服务的前后，只生成一个 `bundle.js` 在客户端。

因为它是一个非常强大的工具，所以我们会常常用到。在开始，Webpack 的功能可能会吓到你，
但我建议你坚持使用下去，一旦你了解了其中的原理，就会觉得得心应手。而你只需给它一个机会去表现。

通常我们不会喜欢那些我们不会的，或是害怕的。然而，一旦你克服初始不适并开始理解它，总会变得很有趣。事实上，这正是我对测试的感受。当开始时讨厌它，在熟悉后喜欢它 :-)

如果感兴趣，这里有一些资源来更多地了解关于 webpack：

1.  [Webpack Cookbook](https://christianalfoni.github.io/react-webpack-cookbook/Getting-started.html)（使用的是 Babel 5，但对于学习 Webpack 的基本原理而言还是很有用的）
2.  [Webpack 初学者可以看这篇文章](http://blog.madewithlove.be/post/webpack-your-bags/)
3.  [Pete Hunts 所写的 Webpack How-to](https://github.com/petehunt/webpack-howto)

> **注意**：如果要持续随本教程实验，建议使用 Node 版本为 `v5.1.0`。当然版本 `>4` 的也是可以的。

首先，安装所有关于 webpack 和 babel 的依赖。Babel 是一个转译器，允许你在开发时使用 ES6（es2015）和 ES7 的特性，然后将这些代码转译成浏览器可以识别的 ES5 代码。

```
mkdir tdd_react
cd tdd_react
npm init        # follow along with normal npm init to set up project

npm i babel-loader babel-core webpack --save-dev
```

> `npm i` 是 npm install 的别名。

接下来，让我们设置项目的路径和创建一个 `webpack.config.js` 文件：

```
mkdir src                  # where all our source code will live
touch src/main.js          # this will be the entry point for our webpack bundling
mkdir test                 # place to store all our tests
mkdir dist                 # this is where the bundled javascript from webpack will go
touch webpack.config.js    # our webpack configuration file
```

初始化的 webpack config 会很小。阅读这些注释，理解下发生了什么：

```
// our webpack.config.js file located in project root

var webpack = require('webpack');
var path = require('path');                // a useful node path helper library

var config = {
  entry: ['./src/main.js'],                // the entry point for our app
  output: {
    path: path.resolve(__dirname, 'dist'), // store the bundled output in dist/bundle.js
    filename: 'bundle.js'                  // specifying file name for our compiled assets
  },
  module: {
    loaders: [
      // telling webpack which loaders we want to use.  For now just run the
      // code through the babel-loader.  'babel' is an alias for babel-loader
      { test: /\.js$/, loaders: ['babel'], exclude: /node_modules/ }
    ]
  }
}

module.exports = config;
```

为了让 babel 更好地工作，我们需要定义哪个 `presets` 是我们需要用到的。让我们继续，并且安装 React 和 ES6 预处理所需的东西：

```
npm i babel-preset-react babel-preset-es2015 --save-dev
```

现在我们有一些选项。在 webpack config 文件中，会告诉你哪一块是做 bebel 预处理的：

```
loaders: [
  {
    test: /\.js$/,
    loaders: ['babel'],
    exclude: /node_modules/,
    query: {
      presets: ['react', 'es2015']
    }
  }
]
```

另外的方法是将他们存在 `.babelrc` 文件中，这也用在我的项目中。将 babel 预处理存储在 `.babelrc` 中，对于以后的开发者而言，更容易去找到哪个 babel 预处理是可用的。此外，当我们将 Karma 设置到 webpack 之后，因为 `.babelrc` 文件的存在，我们就不再需要其他的预处理配置了。

```
# inside our project root
touch .babelrc
```

将下面这段粘贴到预处理文件中：

```
# .babelrc
{
  "presets": ["react", "es2015"]
}
```

为了确认它能否工作，让我们在 `main.js` 中加入一些 react 代码，并看看所有的包是否正常。接着安装 React 和 React DOM：

```
npm i react react-dom -S
```

> 使用 `-S` 是 `--save` 的别名。

创建第一个 React 组件：

```
# src/main.js
import React, { Component } from 'react';
import { render } from 'react-dom';

class Root extends Component {
  render() {
    return <h1> Hello World </h1>;
  }
}

render(<Root />, document.getElementById('root'));
```

聪明的读者就会察觉我们并没有在根部创建一个 `index.html` 文件。让我们继续，当 `bundle.js` 编译后，将其放到 `/dist` 文件夹中：

```html
# /dist/index.html

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8"/>
  </head>
  <body>
    <div id="root"></div>
    <script src="bundle.js"></script>
  </body>
</html>
```

非常棒，让我们继续。最后，我们可以运行 webpack，看看一切是否正常。如果你没有全局安装 webpack（`npm i webpack -g`），你也可以用 node modules 方式进行启动：

```
./node_modules/.bin/webpack
```

Webpack 将默认情况下寻找一个配置名称为 `webpack.config.js`。如果你高兴，也可以通过不同 webpack config 作为参数传入。

在 package.json 中创建一个别名，来完成构建工作：

```
# package.json
... other stuff
"scripts": {
  "build": "webpack"
}
```

接下来让 `webpack-dev-server` 提升开发体验：

```
npm i webpack-dev-server --save-dev
```

将 webpack dev server 的入口加入到 `webpack.config.js` 中：

```
... rest of config
  entry: [
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:3000',
    './src/main.js'
  ],
... rest of config
```

让 script 运行在开发服务器上：

```
# package.json
... other stuff
scripts: {
  "dev": "webpack-dev-server --port 3000 --devtool eval --progress --colors --hot --content-base dist",
  "build": "webpack"
}
```

在 script 中使用了 `--content-base` 标记，告诉 webpack 我们想服务于 `/dist` 文件夹。我们还定义了 3000 端口，使得更像是 Rails 开发的体验。

最后，在 webpack 配置文件中添加一个 resolve 标记，使进口文件看起来更直观。下面就是配置文件最终的样子：

```
var webpack = require('webpack');
var path = require('path');

var config = {
  entry: [
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:3000',
    './src/main.js'
  ],
  resolve: {
    root: [
      // allows us to import modules as if /src was the root.
      // so I can do: import Comment from 'components/Comment'
      // instead of:  import Comment from '../components/Comment' or whatever relative path would be
      path.resolve(__dirname, './src')
    ],
    // allows you to require without the .js at end of filenames
    // import Component from 'component' vs. import Component from 'component.js'
    extensions: ['', '.js', '.json', '.jsx']
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js?$/,
        // dont run node_modules or bower_components through babel loader
        exclude: /(node_modules|bower_components)/,
        // babel is alias for babel-loader
        // npm i babel-core babel-loader --save-dev
        loader: 'babel'
      }
    ],
  }
}

module.exports = config;
```

为确保一切工作正常，让我们运行开发服务器，并且确认我们在屏幕上看到 “Hello World”。

```
npm run dev
open http://localhost:3000
```

你应该看到的是这样的：

![Hello World Image](http://i.imgur.com/rYTjH77.png?1)

## 设置 Mocha，Chai，Sinon 和 Enzyme

**Mocha**：将用于运行我们的测试。
**Chai**：是我们期待的库。应用非常广泛，允许使用 RSpec 一样的语法。
**Sinon**：将服务于 mocks/stubs/spies.
**Enzyme**：将用于测试我们的 React components。AirBnB 写的一个很漂亮的测试库。

安装这些包：

```
npm i mocha chai sinon --save-dev
```

如果我们希望能够使用 ES6 编写测试，那么我们需要在运行前对代码进行转译。那么我们需要安装 babel-register：

```
npm i babel-register --save-dev
```

加一些 npm scripts 到 `package.json` 中，让测试更简单：

```
# ./package.json
... rest of package.json
  "scripts": {
    "test": "mocha --compilers js:babel-register --recursive",
    "test:watch": "npm test -- --watch",
    "build": "webpack",
    "dev": "webpack-dev-server --port 3000 --devtool eval --progress --colors --hot --content-base dist",
  },

```

我们的测试脚本要运行 mocha，并使用 `babel-register` 进行转译，然后递归地查看 `/test` 目录。

最终，我们需要设置 Karma，因此 npm script 会变得无效，但如果不设置，它将会正常工作。`npm run test:watch` 将会监视程序，并在文件发生修改时重新运行。多么高效！

确认它能工作，创建一个 hello world 测试 `/tests/helloWorld.spec.js`：

```
# /test/helloWorld.spec.js
import { expect } from 'chai';

describe('hello world', () => {
  it('works!', () => {
    expect(true).to.be.true;
  });
});
```

哇...看起来很像 RSpec！

如果每一个测试都要引入 `expect`，这将变得很麻烦，因此让我们新建一个 `test_helper` 文件来保存这些东西：

```
# /test/test_helper.js
import { expect } from 'chai';
import sinon from 'sinon';

global.expect = expect;
global.sinon = sinon;
```

然后把它包括到 npm 脚本的运行套件中，并通过 `--require ./test/test_helper.js` 来声明：

```
# package.json script section
  "test": "mocha --compilers js:babel-register --require ./test/test_helper.js --recursive",
```

我也添加了 sinon，因此它也可以全局可用。现在无论什么时候，我们在写一个新的测试时，都不需要手动引入 `expect` 和 `sinon`。

### Enzyme
现在我们所需的“普通”测试工具都已经设置好了（mocha，chai，sinon），接着让我们安装 Enzyme，并且开始测试 React component！

安装这个包：

```
npm i enzyme react-addons-test-utils --save-dev
```

Enzyme 的重要文档可以[在这里找到](http://airbnb.io/enzyme/)。如果有时间，我推荐阅读 Shallow Rendering 部分。
> **你会问，什么是 Shallow Rendering？**

对我们来说是一种组件调用 render 方法，得到我们可以断言的 React 元素，而无需实际安装组件到 DOM 上。更多的 React 元素[请看这](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)。

Enzyme 会将 shallow rendered 组件包裹进一个特殊的 `wrapper` 中，进而让我们可以测试。如果你用过 Rails，这看起来像是 Capybara 中的 `page` 对象。

让我们为一些合适的 `<Root />` 组件进行 TDD 的驱动开发。

这个 Root 组件会是一个 `container`，意味着在应用中它可以控制 state 的处理。学习 React 中“智能”和“笨拙”组件之间的差异，对于应用程序体系结构是很重要的。[这篇文章很好地解释了它们](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.8cnl19w8l)。

```
# /tests/containers/Root.spec.js

import React from 'react';                     // required to get test to work.  we can get around this later with more configuration
import { shallow } from 'enzyme';              // method from enzyme which allows us to do shallow render
import Root from '../../src/containers/Root';  // import our soon to be component

describe('(Container) Root', () => {
  it('renders as a <div>', () => {
    const wrapper = shallow(<Root />);
    expect(wrapper.type()).to.eql('div');
  });

  it('has style with height 100%', () => {
    const wrapper = shallow(<Root />);
    const expectedStyles = {
      height: '100%',
      background: '#333'
    }
    expect(wrapper.prop('style')).to.eql(expectedStyles);
  });

  it('contains a header explaining the app', () => {
    const wrapper = shallow(<Root />);
    expect(wrapper.find('.welcome-header')).to.have.length(1);
  });
});
```

如果我们用 `npm test` 运行测试，这会失败。因为我们没有在适当的位置创建一个根组件。因此我们可以这样做：

> 如果在任何时候你想看到这段代码的源代码，可以在 [github 仓库](https://github.com/SpencerCDixon/react-testing-starter-kit) 中找到

```
# /src/containers/Root.js
import React, { Component } from 'react';

const styles = {
  height: '100%',
  background: '#333'
}

class Root extends Component {
  render() {
    return (
      <div style={styles}>
        <h1 className='welcome-header'>Welcome to testing React!</h1>
      </div>
    )
  }
}

export default Root;
```
重新运行测试就可以了。

在我们的测试中有很多重复的东西，因此我们还需要回去做一些重构。由于我们没有给 `Root` 传入任何的 props，那么我们可以 shallow render 它一次，然后就在一个 wrapper 中结束了我们所有的断言。很多时候给定一个特定的 props 后，我发现自己包装的部分测试会在 “sub” describe 块中，然后给一堆断言也有这些 props。如果你用过 RSpec，就类似于使用 “context” 块。

```
describe('(Container) Root', () => {
  const wrapper = shallow(<Root />);

  it('renders as a <div>', () => {
    expect(wrapper.type()).to.eql('div');
  });

  it('has style with height 100%', () => {
    const expectedStyles = {
      height: '100%',
      background: '#333'
    }
    expect(wrapper.prop('style')).to.eql(expectedStyles);
  });

  it('contains a header explaining the app', () => {
    expect(wrapper.find('.welcome-header')).to.have.length(1);
  });
});
```

尽可能地在你的测试中使用 `shallow`，但偶尔也可能不用。例如，如果你要测试 React 生命周期的方法时，就需要真正地将组件安装出来。

接下来让我们测试一个组件的安装和调用函数，当它安装时，我们可以得到一些暴露在 `sinon` 上的信息和正在使用的 spies。

我们可以假装 `Root` 组件有一个子组件叫 `CommentList`，在安装后将调用任意的回调。当通过给定 props 组件安装时，函数被调用，因此我们就可以测试这个场景。在组件渲染时给评论列表一些 style，然后我们就可以知道 shallow render 是如何处理这些样式的了。`CommentList` 会在一个组件文件夹的 `/src/components/CommentList.js` 中。因为它不处理数据，因此完全取决于 props，换句话说它是一个**纯**（**笨拙**）组件：

```
import React from 'react';

// Once we set up Karma to run our tests through webpack
// we will no longer need to have these long relative paths
import CommentList from '../../src/components/CommentList';
import {
  describeWithDOM,
  mount,
  shallow,
  spyLifecycle
} from 'enzyme';

describe('(Component) CommentList', () => {

  // using special describeWithDOM helper that enzyme
  // provides so if other devs on my team don't have JSDom set up
  // properly or are using old version of node it won't bork their test suite
  //
  // All of our tests that depend on mounting should go inside one of these
  // special describe blocks
  describeWithDOM('Lifecycle methods', () => {
    it('calls componentDidMount', () => {
      spyLifecyle(CommentList);

      const props = {
        onMount: () => {},  // an anonymous function in ES6 arrow syntax
        isActive: false
      }

      // using destructuring to pass props down
      // easily and then mounting the component
      mount(<CommentList {...props} />);

      // CommentList's componentDidMount should have been
      // called once.  spyLifecyle attaches sinon spys so we can
      // make this assertion
      expect(
        CommentList.prototype.componentDidMount.calledOnce
      ).to.be.true;
    });

    it('calls onMount prop once it mounts', () => {
      // create a spy for the onMount function
      const props = { onMount: sinon.spy() };

      // mount our component
      mount(<CommentList {...props} />);

      // expect that onMount was called
      expect(props.onMount.calledOnce).to.be.true;
    });
  });
});
```

还有很多，阅读这些注释可以帮助你更好地理解。看看这些实践，让测试可以通过，然后再回头看看这些测试，验证下你所理解的东西。

```
# /src/components/CommentList.js
import React, { Component, PropTypes } from 'react';

const propTypes = {
  onMount: PropTypes.func.isRequired,
  isActive: PropTypes.bool
};

class CommentList extends Component {
  componentDidMount() {
    this.props.onMount();
  }

  render() {
    return (
      <ul>
        <li> Comment One </li>
      </ul>
    )
  }
}

CommentList.propTypes = propTypes;
export default CommentList;
```

运行 `npm test` ，现在这些套件应该可以通过测试了。

接下来让我们添加一些 shallow rendered 测试，当给定一个 `isActive` 的 props 时，来确保我们的组件使用了适当的 CSS class。

```
... previous tests

  it('should render as a <ul>', () => {
    const props = { onMount: () => {} };
    const wrapper = shallow(<CommentList  {...props} />);
    expect(wrapper.type()).to.eql('ul');
  });

  describe('when active...', () => {
    const wrapper = shallow(
      // just passing isActive is an alias for true
      <CommentList onMount={() => {}} isActive />
    )
    it('should render with className active-list', () => {
      expect(wrapper.prop('className')).to.eql('active-list');
    });
  });

  describe('when inactive...', () => {
    const wrapper = shallow(
      <CommentList onMount={() => {}} isActive={false} />
    )
    it('should render with className inactive-list', () => {
      expect(wrapper.prop('className')).to.eql('inactive-list');
    });
  });
});
```

让它们通过测试：

```
class CommentList extends Component {
  componentDidMount() {
    this.props.onMount();
  }

  render() {
    const { isActive } = this.props;
    const className = isActive ? 'active-list' : 'inactive-list';

    return (
      <ul className={className}>
        <li> Comment One </li>
      </ul>
    )
  }
}
```

此时你应该对如何测试 react 组件已经有了一个很好的理解了。记得去阅读 Enzyme 文档来获得更多的灵感。

## 设置 Karma
设置 Karma 可能会有些困难。坦白讲，这对我而言也是一件痛苦的工作。通常，当我开发 React 应用时，我会选择使用已经构建好的 starter kit，方便省事。[我非常推荐开发时用的 starter kit](https://github.com/davezuko/react-redux-starter-kit)。

使用 Karma 的价值在于快速测试重载，可以多浏览器测试和最重要的是 webpack 预处理。一旦我们将 Karma 设置好了，在我们运行测试程序时，不仅是只有 `babel-loader`，而是整个 webpack config。这为我们提供了很多便利，使得我们的测试环境与开发环境相同。

让我们开始吧...

```
npm i karma karma-chai karma-mocha karma-webpack --save-dev
npm i karma-sourcemap-loader karma-phantomjs-launcher --save-dev
npm i karma-spec-reporter --save-dev
npm i phantomjs --save-dev

# The polyfills arn't required but will help with browser support issues
# and are easy enough to include in our karma config that I figured why not
npm i babel-polyfill phantomjs-polyfill --save-dev
```

很多包，我知道。相信我完成这个是非常值得的。

对于我们的示例而言，我们将使用 [PhantomJS](http://phantomjs.org/)。没有别的什么原因，这我在 starter kit 中已经用到了。可以按照自己的喜好使用 Chrome，Firefox 或是 Safari，甚至在 PhantomJS 之上。（这是用 Karma 的一件很酷的事）

在配置 karma 之前先安装 `yargs`，它能让你使用命令行参数来定制 Karma 的配置。

```
npm i yargs -S
```

现在我们可以通过创建一个 Karma config 文件去监视我们的文件，当文件发生修改时重新运行并很快地保存。

**Karma Config**：
```
touch karma.config.js
```

```
// ./karma.config.js

var argv = require('yargs').argv;
var path = require('path');

module.exports = function(config) {
  config.set({
    // only use PhantomJS for our 'test' browser
    browsers: ['PhantomJS'],

    // just run once by default unless --watch flag is passed
    singleRun: !argv.watch,

    // which karma frameworks do we want integrated
    frameworks: ['mocha', 'chai'],

    // displays tests in a nice readable format
    reporters: ['spec'],

    // include some polyfills for babel and phantomjs
    files: [
      'node_modules/babel-polyfill/dist/polyfill.js',
      './node_modules/phantomjs-polyfill/bind-polyfill.js',
      './test/**/*.js' // specify files to watch for tests
    ],
    preprocessors: {
      // these files we want to be precompiled with webpack
      // also run tests throug sourcemap for easier debugging
      ['./test/**/*.js']: ['webpack', 'sourcemap']
    },
    // A lot of people will reuse the same webpack config that they use
    // in development for karma but remove any production plugins like UglifyJS etc.
    // I chose to just re-write the config so readers can see what it needs to have
    webpack: {
       devtool: 'inline-source-map',
       resolve: {
        // allow us to import components in tests like:
        // import Example from 'components/Example';
        root: path.resolve(__dirname, './src'),

        // allow us to avoid including extension name
        extensions: ['', '.js', '.jsx'],

        // required for enzyme to work properly
        alias: {
          'sinon': 'sinon/pkg/sinon'
        }
      },
      module: {
        // don't run babel-loader through the sinon module
        noParse: [
          /node_modules\/sinon\//
        ],
        // run babel loader for our tests
        loaders: [
          { test: /\.js?$/, exclude: /node_modules/, loader: 'babel' },
        ],
      },
      // required for enzyme to work properly
      externals: {
        'jsdom': 'window',
        'cheerio': 'window',
        'react/lib/ExecutionEnvironment': true,
        'react/lib/ReactContext': 'window'
      },
    },
    webpackMiddleware: {
      noInfo: true
    },
    // tell karma all the plugins we're going to be using to prevent warnings
    plugins: [
      'karma-mocha',
      'karma-chai',
      'karma-webpack',
      'karma-phantomjs-launcher',
      'karma-spec-reporter',
      'karma-sourcemap-loader'
    ]
  });
};
```
阅读所有的注释一次或两次有助于理解这个 config 是做什么的。使用 Webpack 的一大好处是全部都是普通的 JavaScript 代码，并且我们可以“重构”配置文件。事实上，这正是绝大多数 starter kit 所做的！

随着 Karma 设置完成，为运行测试，最后一件事就是要去更新我们的 package.json：

```
# package.json
  "scripts" {
    "test": "node_modules/.bin/karma start karma.config.js",
    "test:dev": "npm run test -- --watch",
    "old_test": "mocha --compilers js:babel-register --require ./test/test_helper.js --recursive",
    "old_test:watch": "npm test -- --watch"
  }
```

我建议重命名旧的测试 scripts 的前缀，用 'old_' 表示。

最终的 `package.json` 是这样的：

```
{
  "name": "react-testing-starter-kit",
  "version": "0.1.0",
  "description": "React starter kit with nice testing environment set up.",
  "main": "src/main.js",
  "directories": {
    "test": "tests",
    "src": "src",
    "dist": "dist"
  },
  "dependencies": {
    "react": "^0.14.6",
    "react-dom": "^0.14.6",
    "yargs": "^3.31.0"
  },
  "devDependencies": {
    "babel-core": "^6.4.0",
    "babel-loader": "^6.2.1",
    "babel-polyfill": "^6.3.14",
    "babel-preset-es2015": "^6.3.13",
    "babel-preset-react": "^6.3.13",
    "babel-register": "^6.3.13",
    "chai": "^3.4.1",
    "enzyme": "^1.2.0",
    "json-loader": "^0.5.4",
    "karma": "^0.13.19",
    "karma-chai": "^0.1.0",
    "karma-mocha": "^0.2.1",
    "karma-phantomjs-launcher": "^0.2.3",
    "karma-sourcemap-loader": "^0.3.6",
    "karma-spec-reporter": "0.0.23",
    "karma-webpack": "^1.7.0",
    "mocha": "^2.3.4",
    "phantomjs": "^1.9.19",
    "phantomjs-polyfill": "0.0.1",
    "react-addons-test-utils": "^0.14.6",
    "sinon": "^1.17.2",
    "webpack": "^1.12.11",
    "webpack-dev-server": "^1.14.1"
  },
  "scripts": {
    "test": "node_modules/.bin/karma start karma.config.js",
    "test:dev": "npm run test -- --watch",
    "build": "webpack",
    "dev": "webpack-dev-server --port 3000 --devtool eval --progress --colors --hot --content-base dist",
    "old_test": "mocha --compilers js:babel-register --require ./test/test_helper.js --recursive",
    "old_test:watch": "npm test -- --watch"
  },
  "repository": {
    "type": "git",
    "url": "tbd"
  },
  "author": "Spencer Dixon",
  "license": "ISC"
}
```

在测试套件中外加 webpack 预处理，我们现在可以删除那些在测试内烦人的相对路径声明：

```
// test/containers/Root.spec.js
import React from 'react';
import { shallow } from 'enzyme';
import Root from 'containers/Root';               // new import statement
// import Root from '../../src/containers/Root';  // old import statement

// test/components/CommentList.spec.js
import React from 'react';
import CommentList from 'components/CommentList';               // new import statement
// import CommentList from '../../src/components/CommentList';  // old import statement

import {
  describeWithDOM,
  mount,
  shallow,
  spyLifecycle
} from 'enzyme';

```

现在使用这个 starter kit 开发，你需要输入以下这些命令去运行程序：

```
npm run dev         # note the addition of run
npm run test:dev    # note the addition of run
```

[如果还有什么不清楚的地方，可以在 github 上查看该源码](https://github.com/SpencerCDixon/react-testing-starter-kit)。

### 结论

我们已经建立了一个坚实的测试环境，可以根据你的项目具体需求去改变和发展。在下一次的文章中，我将花更多的时间在特殊场景的测试，还有如何测试 Redux，我更喜欢 flux 的实现。

虽然我只使用 React 开发了数月，但我已经爱上它了。我希望本教程可以帮助你更深入地理解一些 React 测试的最佳实践。有任何问题或评论随时联系我。测试是我们的好朋友！

[**原文链接**](http://spencerdixon.com/blog/test-driven-react-tutorial.html?utm_campaign=Front%2BEnd%2BNewsletter&utm_medium=email&utm_source=Front_End_Newsletter_2)
