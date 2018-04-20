通过原生 JS 实现了全屏滚动：兼容 IE 10+、手机触屏，Mac 触摸板优化，支持自定义页面动画，压缩后代码只有 2k。

查看 [DEMO](https://xiaogliu.github.io/pure_full_page/index.html)

## 前面的话

现在已经有很多全屏滚动插件了，比如著名的 [fullPage](https://github.com/alvarotrigo/fullPage.js)，那为什么还要自己造轮子呢？

首先，现有轮子有以下问题：

* 首先，最大的问题是最流行的几个插件都依赖 jQuery，这意味着在使用 React 或者 Vue 的项目中使用是一件十分蛋疼的事：我只需要一个全屏滚动功能，却还需要把 jQuery 引入，有种杀鸡使用宰牛刀的感觉；
* 其次，现有的很多全屏滚动插件功能往往都十分丰富，这在前几年是优势，但现在（2018-4）可以看作是劣势：前端开发已经发生了很大变化，其中很重要的一个变化是 ES6 原生支持模块化开发，模块化开发最大的特点是一个模块最好只专注做好一件事，然后再拼成一个完整的系统，从这个角度看，大而全的插件有悖模块化开发的原则。

对比之下，通过原生语言造轮子有以下好处：

* 使用原生语言编写的插件，自身不会受依赖的插件的使用场景而影响自身的使用（现在依赖 jQuery 的插件非常不适合开发单页面应用），所以使用上更加灵活；
* 搭配模块化开发，使用原生语言开发的插件可以只专注一个功能，所以代码量可以很少（我编写的这个全屏滚动插件去掉注释和空行后，全部代码不到 100 行）；
* 最后，随着 JS/CSS/HTML 的发展以及浏览器不断迭代更新，现在使用原生语言编写插件的开发成本越来越低，那为什么不呢？

## 使用方法

### 1） 创建 html，结构如下

```html
<div id="pureFullPage">
  <div class="page"></div>
  <div class="page"></div>
  <div class="page"></div>
</div>
```

其中，id 为 `pureFullPage` 的 div 是所有滚动页面的容器，class 为 `page` 的 div 为具体页面的容器。

页面容器 id 必须为 `pureFullPage`，具体页面 class 必须包含 `page`，因为 css 会根据 `#pureFullPage` 和 `.page` 设置样式。

### 2）引入 pureFullPage 的 JS 和 CSS 文件

pureFullPage 的 JS 和 CSS 压缩后的文件在 `dist` 目录下，源文件在 `src` 目录下。

* 传统引入方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    ...
    <link rel="stylesheet" href="youtpath/pureFullPage.min.css">
</head>
<body>
    ...
    <script src="youtpath/pureFullPage.min.js"></script>
</body>
</html>
```

* ES6 模块化引入

npm package 开发中，敬请期待。。。

> 不闲麻烦，可以将 `src` 目录下的源文件手动引入项目...

### 3）新建 pureFullPage 实例并初始化

```js
// 创建实例并初始化
new PureFullPage().init();
```

### 4）自定义参数

实例化 pureFullPage 时接受一个对象作为参数，可以控制是否显示右侧导航（移动端往往不需要右侧导航）及自定义页面动画，实例代码如下：

```js
// 创建实例并初始化
new PureFullPage({
  isShowNav: false,
  definePages: addAnimation,
}).init();
```

其中：

* `isShowNav` 控制是否显示右侧导航，Boolean 类型，默认为 `true`，设为 `false` 则不显示；

* `definePages` 是 Function 类型，默认为空函数（什么都不操作）。这个函数会在页面每次滚动时触发，主要用来**获取将要进入的页面元素**，拿到页面元素，就可以进行相关操作了，一般是添加动画。

`definePages` 需要特别说明下，若需要自定义，则**定义时不能使用箭头函数**，因为自定义函数内部 `this` 在 `pureFullPage` 实现时绑定到了实例本身，方便获取将要进入的页面元素，见下面为将要进入的页面添加动画的示例代码：

```js
// 不能使用箭头函数，要引用实例中的 this
let addAnimation = function() {
  // i 表示每次滑动将要进入的页面的索引，可以通过 this.pages[i] 获取当前页面元素
  // 取得将要进入的页面元素后便可以做进一步操作
  let i = -(this.currentPosition / this.viewHeight);

  // 为将要进入页面添加动画
  document.querySelector('.fade-in').classList.remove('fade-in');
  this.pages[i].querySelector('p').classList.add('fade-in');
};
```

> 添加动画的完整代码在 `demo/add_animation/` 目录下

## 了解更多

可查看这篇文章查看详细开发过程（待补充）

## TODO

1.  详细介绍文章；
2.  npm package；
3.  英文版说明。
