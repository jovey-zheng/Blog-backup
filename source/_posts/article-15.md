title: CSS 的使用中你可能不知道的 7 件事
date: 2015-12-22 15:50:56
categories:
  - 好文分享
tags:
  - css
  - features
  - front-end
---

无论你信不信，JavaScript 和 CSS 已经开始重叠，并为 CSS 增加了更多的功能。当我在写 [CSS 与 JavaScript 交互中你可能不知道的 5 种方式](https://davidwalsh.name/ways-css-javascript-interact)的时候，读者对于 CSS 与 JavaScript 已经重叠的事感到很惊讶。那么今天，我会着重为你介绍 7 个可以通过 CSS 完成的任务 —— 不使用 JavaScript 或图像处理！

# [CSS @supports](https://davidwalsh.name/css-supports)

每个优秀的前端工程师都会在使用某个特性前测试一下，确保是否在浏览器中可以使用。而这类测试通常是由 JavaScript 完成的，当然，也有很多人用 Modernizr（拥有很多很好功能的测试工具）来测试特性。那么现在有一个 CSS 新的 API 可以让你去做特性测试：[@supports](https://davidwalsh.name/css-supports)，下面的例子将简单地教你如何使用：

```css
/* basic usage */
@supports(prop:value) {
  /* more styles */
}

/* real usage */
@supports (display: flex) {
  div { display: flex; }
}

/* testing prefixes too */
@supports (display: -webkit-flex) or
          (display: -moz-flex) or
          (display: flex) {

    section {
      display: -webkit-flex;
      display: -moz-flex;
      display: flex;
      float: none;
    }
}
```

<!-- more -->

@supports 这个新的特性也有一个相对应的 JavaScript 方式，但这个特性还在实验阶段，希望我们可以很快用到！

# [CSS Filters](https://davidwalsh.name/css-filters)

你可以写一个服务去修改图片的颜色，同时还可以把它卖给 Facebook 获得大量的钱！当然，写一个图片过滤器功能只是一个简单化的实现，并非一门科学。在 Mozilla 的第一周，我写了一个[小的应用](https://github.com/darkwing/fotofilter)（这让我赢了比赛，BTW...就说说而已），这个应用使用了一些 JS-base 的数学方法并使用 canvas 去做图片过滤器。这很麻烦，但是现在我们可以[用 CSS 的特性去完成这一功能](https://davidwalsh.name/css-filters)！

```css
/* simple filter */
.myElement {
  -webkit-filter: blur(2px);
}

/* advanced filter */
.myElement {
  -webkit-filter: blur(2px) grayscale (.5) opacity(0.8) hue-rotate(120deg);
}
```

这类的过滤功能只是创建一个图片的原型，并且不会保存和导出来完成过滤器的功能。这对于图片管理或想处理任何一张图片来说很方便！

 * [Demo](https://davidwalsh.name/demo/css-filters.php)

# [Pointer Events 和点击事件](https://davidwalsh.name/pointer-events)

[CSS 特性中的 `pointer-events`](https://davidwalsh.name/pointer-events) 提供了一个方法，能使一个元素 disable，即在用户点击某个元素时，不触发它在 JavaScript 中写的点击事件：

```css
/* do nothing when clicked or activated */
.disabled { pointer-events: none; }
```

```js
/* this will _not_ fire because of the pointer-events: none application */
document.getElementById("disabled-element").addEventListener("click", function(e) {
  alert("Clicked!");
});
```

在上述的例子中，由于 `pointer-events` 的值是 `none`，而使点击事件不被触发。大有用处的是，让你不必到处去检查类名或属性，来确认哪一个是 disabled 的了。

 * [Demo](https://davidwalsh.name/demo/pointer-events.php)

# [Slide Down & Slide Up](https://davidwalsh.name/css-slide)

CSS 使我们能够创建转换和动画，但通常我们需要一个 JavaScript 库帮助我们实现。例如一个比较流行的动画效果[ slide up 和 silde down](https://davidwalsh.name/css-slide)，大概很多人都不知道这个可以只用 CSS 实现吧！

```css
/* slider in open state */
.slider {
  overflow-y: hidden;
  max-height: 500px; /* approximate max height */

  transition-property: all;
  transition-duration: .5s;
  transition-timing-function: cubic-bezier(0, 1, 0.5, 1);
}

/* close it with the "closed" class */
.slider.closed {
  max-height: 0;
}
```

很聪明地使用了 `max-height` 来控制元素的展开和收缩。

 * [Demo](https://davidwalsh.name/demo/css-slide.php)

# [CSS Counters](https://davidwalsh.name/css-counters)

我们不禁地问，“counter” 在网上意味着什么呢？但是 `CSS Counters` 就是另外一回事了。这个特性可以把一个 counter 加到元素中，通过 `:before` 或 `:after` ：

```css
/* initialize the counter */
ol.slides {
  counter-reset: slideNum;
}

/* increment the counter */
ol.slides > li {
  counter-increment: slideNum;
}

/* display the counter value */
ol.slides li:after {
  content: "[" counter(slideNum) "]";
}
```

通常这个会在一些模块或是列表中用到。

 * [Demo](https://davidwalsh.name/demo/css-counters.php)

# [Unicode CSS Classes](https://davidwalsh.name/unicode-css-classes)

有大量的文章说明，去教你如何对 CSS 的类命名。但你应该不知道会有这样的文档，去教你[用特殊字符命名你的 css 类](https://davidwalsh.name/unicode-css-classes)：

```css
.ಠ_ಠ {
  border: 1px solid #f00;
  background: pink;
}

.❤ {
  background: lightgreen;
  border: 1px solid green;
}
```

但请不要这么使用。

 * [Demo](https://davidwalsh.name/demo/unicode-css-classes.php)

# [CSS Circles](https://davidwalsh.name/css-circles)

[CSS 中的圆形](https://davidwalsh.name/css-circles) 与 [CSS 中的三角形](https://davidwalsh.name/css-triangles)一样。通过使用 `border-radius` 就能创建一个完美的圆形！

```css
.circle {
  border-radius: 50%;
  width: 200px;
  height: 200px;
  /* width and height can be anything, as long as they're equal */
}
```

你也可以给圆形添加一些渐变效果，甚至可以添加动画。CSS 拥有更多[对于图形统一的 API](http://alistapart.com/article/css-shapes-101)，当然现在你可以使用 hack 创建一个圆形。

 * [Demo](https://davidwalsh.name/demo/css-circles.php)

就是这些：你可能不知道的 7 个关于 CSS 的事，一部分是临界的情况，其他一部分还是非常实用的。那么就大胆地去用它们吧！

[原文链接](https://davidwalsh.name/css-facts)
