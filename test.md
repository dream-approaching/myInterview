<!-- TOC -->

- [常见面试题](#常见面试题)
  - [1.如何解决 a 标签点击后 hover 事件失效的问题?](#1如何解决-a-标签点击后-hover-事件失效的问题)
  - [2.常见手写函数](#2常见手写函数)
    - [闭包](#闭包)
    - [柯里化](#柯里化)
    - [防抖](#防抖)
    - [节流](#节流)
    - [深拷贝](#深拷贝)
    - [继承](#继承)
    - [new 内部原理](#new-内部原理)
    - [实现一个 ajax](#实现一个-ajax)
  - [3.点击一个 input 依次触发的事件](#3点击一个-input-依次触发的事件)
  - [4. Vue/React 中 hash 模式和 history 模式的区别](#4-vuereact-中-hash-模式和-history-模式的区别)
  - [5.数组去重](#5数组去重)
  - [6.addEventListener 函数的第三个参数](#6addeventlistener-函数的第三个参数)
  - [7.所有的事件都有冒泡吗？](#7所有的事件都有冒泡吗)
  - [8.typeof 和 instanceof 的区别](#8typeof-和-instanceof-的区别)
  - [9.在移动端中怎样初始化根元素的字体大小](#9在移动端中怎样初始化根元素的字体大小)
  - [10.移动端布局](#10移动端布局)
  - [11.知道 meta 标签有把 http 换成 https 的功能吗](#11知道-meta-标签有把-http-换成-https-的功能吗)
  - [12.性能优化](#12性能优化)
    - [12.1 React 中怎么优化性能](#121-react-中怎么优化性能)
  - [13.webpack](#13webpack)
    - [13.1 webpack 和 gulp 区别](#131-webpack-和-gulp-区别)
    - [13.2 webpack 怎么打包多页面](#132-webpack-怎么打包多页面)
    - [13.3 有哪些常见的 Loader？你用过哪些 Loader？](#133-有哪些常见的-loader你用过哪些-loader)
    - [13.4 有哪些常见的 Plugin？你用过哪些 Plugin？](#134-有哪些常见的-plugin你用过哪些-plugin)
    - [13.5 说一说 Loader 和 Plugin 的区别?](#135-说一说-loader-和-plugin-的区别)
    - [13.6 Webpack 构建流程简单说一下](#136-webpack-构建流程简单说一下)
    - [13.7 如何优化 Webpack 的构建速度？](#137-如何优化-webpack-的构建速度)
    - [13.8 什么是 Tree shaking？如何开启](#138-什么是-tree-shaking如何开启)
    - [13.9 如何实现代码分割](#139-如何实现代码分割)
    - [13.10 webpack 中如何处理图片的](#1310-webpack-中如何处理图片的)
  - [14 slice、substr 和 substring 有什么区别](#14-slicesubstr-和-substring-有什么区别)
  - [15. react-redux connect 的原理是什么](#15-react-redux-connect-的原理是什么)
  - [16.HTML5 有哪些新特性](#16html5-有哪些新特性)
  - [17. CSS3 有哪些新特性](#17-css3-有哪些新特性)
  - [18. ES6 有哪些新特性](#18-es6-有哪些新特性)
  - [19. 常用浏览器内核以及 css 前缀](#19-常用浏览器内核以及-css-前缀)
  - [git-flow 有了解过吗](#git-flow-有了解过吗)

<!-- /TOC -->

## 常见面试题

### 1.如何解决 a 标签点击后 hover 事件失效的问题?

改变 a 标签 css 属性的排列顺序,只需要记住`LoVe HAte`原则就可以了：

```
link→visited→hover→active
```

比如下面错误的代码顺序：

```css
a:hover {
  color: green;
  text-decoration: none;
}
a:visited {
  /* visited在hover后面，这样的话hover事件就失效了 */
  color: red;
  text-decoration: none;
}
```

### 2.常见手写函数

#### 闭包

```js
function showName() {
  const firstName = "zheng";
  return function() {
    console.log(`${firstName} longzi`);
  };
}
const fn = showName();
fn(); // zheng longzi
```

#### 柯里化

```js
function curry(fn, args1 = []) {
  const { length } = fn;
  return function(...args2) {
    const args = [...args1, ...args2];
    if (args.length < length) {
      return curry.call(this, fn, args);
    }
    return fn.apply(this, args);
  };
}
```

#### 防抖

```js
function debounce(fn, ms) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, ms);
  };
}
```

#### 节流

```js
function throttle = (fn, ms) => {
  let timer;
  let lastTime = Date.now();
  let canRun = true;
  return function(...args) {
    if (canRun) {
      fn.apply(this, args)
      canRun = false;
    } else {
      const disTime = Date.now() - lastTime
      clearTimeout(timer)
      timer = setTimeout(() => {
        if (disTime >= ms) {
          fn.apply(this, args)
          lastTime = Date.now()
        }
      }, Math.max(ms - disTime, 0))
    }
  }
}
```

#### 深拷贝

```js
function deepClone(obj) {
  let clone = Object.assign({}, obj);
  Object.keys(clone).forEach((item) => {
    if (typeof clone[item] === "object") {
      clone[item] = deepClone(clone[item]);
    }
    clone[item] = clone[item];
  });
  if (Array.isArray(obj)) {
    clone.length = obj.length;
    return Array.from(clone);
  }
  return clone;
}
```

#### 继承

```js
function Parent() {}
function Child(...args) {
  Parent.apply(this, args);
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
```

#### new 内部原理

```js
var a = new myFunction("Li","Cherry");
new MyFunction {
  const obj = {}
  obj.__proto__ = MyFunction.prototype;
  const result = MyFunction.call(obj, "Li","Cherry")
  return typeof result === 'object' ? result : obj;
}
```

#### 实现一个 ajax

```js
var xhr = new XMLHttpRequest()
// 必须在调用 open()之前指定 onreadystatechange 事件处理程序才能确保跨浏览器兼容性
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status ==== 304) {
      console.log(xhr.responseText)
    } else {
      console.log('Error:' + xhr.status)
    }
  }
}
// 第三个参数表示异步发送请求
xhr.open('get', '/api/getSth',  true)
// 参数为作为请求主体发送的数据
xhr.send(null)
```

### 3.点击一个 input 依次触发的事件

```js
const text = document.getElementById("text");
text.onclick = function(e) {
  console.log("onclick");
};
text.onfocus = function(e) {
  console.log("onfocus");
};
text.onmousedown = function(e) {
  console.log("onmousedown");
};
text.onmouseenter = function(e) {
  console.log("onmouseenter");
};

// 'nmouseenter => onmousedown => onfocus => onclick
```

### 4. Vue/React 中 hash 模式和 history 模式的区别

- 最明显的是在显示上，hash 模式的 URL 中会夹杂着#号，而 history 没有。
- hash 模式是依靠 onhashchange 事件(监听 location.hash 的改变)，而 history 模式是主要是依靠的 HTML5 history 的方法
- 这时候 history 模式需要后端的支持。因为 history 模式下，前端的 URL 必须和实际向后端发送请求的 URL 一致。所以需要后端增加一个覆盖所有情况的候选资源，一般会配合前端给出的一个 404 页面。

### 5.数组去重

- `Array.from(new Set(arr))` / `[...new Set(arr)]`
- 利用 includes
  ```js
  function unique(arr) {
    let res = [];
    for (let i = 0; i < arr.length; i++) {
      if (!res.includes(arr[i])) {
        res.push(arr[i]);
      }
    }
    return res;
  }
  ```
  > 扩展：Set 取交集 并集 差集

```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集 Set {1, 2, 3, 4}
let union = new Set([...a, ...b]);

// 交集 set {2, 3}
let intersect = new Set([...a].filter((x) => b.has(x)));

// （a 相对于 b 的）差集 Set {1}
let difference = new Set([...a].filter((x) => !b.has(x)));
```

### 6.addEventListener 函数的第三个参数

第三个参数涉及到冒泡和捕获，是 true 时为捕获，是 false 则为冒泡 默认值。

或者是一个对象{passive: true}，针对的是 Safari 浏览器，禁止/开启使用滚动的时候要用到。

### 7.所有的事件都有冒泡吗？

并不是所有的事件都有冒泡的，例如以下事件就没有：

- onblur
- onfocus
- onmouseenter
- onmouseleave

### 8.typeof 和 instanceof 的区别

typeof 表示是对某个变量类型的检测，基本数据类型除了 null 都能正常的显示为对应的类型，引用类型除了函数会显示为'function'，其它都显示为 object。

instanceof 主要是用于实例的判断。 `A instanceof B` 用来判断 A 是否为 B 的实例。也可以判断一个实例是否是其父类型或者祖先类型的实例

> 扩展：变量类型

- 基本类型： string, number, boolean, undefined, null, Symbol, bigInt(es10 新增)
- 引用类型： function, array, object

### 9.在移动端中怎样初始化根元素的字体大小

- 动态计算 font-size
  ```js
  (function() {
    var html = document.documentElement;
    function onWindowResize() {
      html.style.fontSize = html.getBoundingClientRect().width / 20 + "px";
    }
    window.addEventListener("resize", onWindowResize);
    onWindowResize();
  })();
  ```
- 还需要配合一个 meta 头
  `<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-sacle=1.0, maximum-scale=1.0, user-scalable=no" />`

### 10.移动端布局

- 移动端布局的方式主要使用 rem 和 flex，可以结合 rem 和媒体查询，然后不同的上视口大小下设置设置 html 的 font-size。
- 可单独制作移动端页面也可响应式 pc 端移动端共用一个页面。没有好坏，视情况而定，因势利导
- 我一般是结合两个库`postcss-pxtorem`和`lib-flexible`，前者用于转换单位，后者用于修改根节点字体大小

### 11.知道 meta 标签有把 http 换成 https 的功能吗

利用 meta 标签把 http 请求换为 https

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" />
```

### 12.性能优化

- 降低请求量：合并资源，减少 HTTP 请求数，minify / gzip 压缩，webP，图片 lazyLoad。
- 加快请求速度：预解析 DNS，减少域名数，并行加载，CDN 分发。
- 缓存：HTTP 协议缓存请求，离线数据缓存 localStorage。
- 渲染：JS/CSS 优化（避免使用 CSS 表达式），加载顺序（将 CSS 样式表放在顶部，把 javascript 放在底部），服务端渲染，pipeline。

#### 12.1 React 中怎么优化性能

主要是指代码层面上

- 使用纯组件
- 函数组件可以使用 React.memo
- shouldComponentUpdate
- 懒加载组件：用 Suspense 和 lazy
- 使用 React Fragments
- 组件的复用：高阶组件，render props
- 避免使用内联函数
- 避免使用内联样式
- 不要在 render 中改变数据
- 加唯一 key 值
- 高频事件节流和防抖
- 用 CSS 动画代替 JavaScript 动画
- React 组件的服务端渲染

### 13.webpack

#### 13.1 webpack 和 gulp 区别

- gulp 强调的是前端开发的工作流程，我们可以通过配置一系列的 task，定义 task 处理的事务
- webpack 是一个前端模块化方案，更侧重模块打包

#### 13.2 webpack 怎么打包多页面

需要在单页面的基础上改动 2 个地方

- entry 中添加每个页面对应的入口文件
- html-webpack-plugin 添加每个页面对应的 html 模板

#### 13.3 有哪些常见的 Loader？你用过哪些 Loader？

- `file-loader`：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
- `url-loader`：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
- `image-loader`：加载并且压缩图片文件
- `babel-loader`：把 ES6 转换成 ES5
- `json-loader` 加载 JSON 文件（默认包含）
- `css-loader`：加载 CSS，支持模块化、压缩、文件导入等特性
- `style-loader`：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
- `postcss-loader`：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀

> 说几个就好

#### 13.4 有哪些常见的 Plugin？你用过哪些 Plugin？

- `define-plugin`：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
- `html-webpack-plugin`：简化 HTML 文件创建 (依赖于 html-loader)
- `uglifyjs-webpack-plugin`：不支持 ES6 压缩 (Webpack4 以前)
- `terser-webpack-plugin`: 支持压缩 ES6 (Webpack4)
- `clean-webpack-plugin`: 目录清理
- `webpack-bundle-analyzer`: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)

#### 13.5 说一说 Loader 和 Plugin 的区别?

- Loader
  - 本质是一个函数，在该函数中对接收到的内容进行转换
  - 在 module.rules 中配置
- Plugin
  - 是插件，可以扩展 Webpack 的功能
  - 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
  - 在 plugins 中单独配置

#### 13.6 Webpack 构建流程简单说一下

- 初始化参数
- 开始编译 => 确定入口 => 编译模块 => 完成模块编译
- 输出资源 => 输出完成

#### 13.7 如何优化 Webpack 的构建速度？

- 使用高版本的 Webpack 和 Node.js
- 多进程/多实例构建
- 压缩代码
- 图片压缩
- 缩小打包作用域
- 提取页面公共资源
- Tree shaking

#### 13.8 什么是 Tree shaking？如何开启

- Tree-shaking 是指在打包中取出那些引入了但在代码中没有被用到的死代码。
- webpack 中通过 uglifysPlugin 来 Tree-shaking JS。

#### 13.9 如何实现代码分割

有三种方式

- 入口配置：entry 入口使用多个入口文件；
- 抽取公有代码：使用 SplitChunks 抽取公有代码；
- 动态加载 ：动态加载一些代码。

#### 13.10 webpack 中如何处理图片的

在 webpack 中有两种处理图片的 loader：

file-loader：解决 CSS 等中引入图片的路径问题；(解决通过 url,import/require()等引入图片的问题)
url-loader：当图片小于设置的 limit 参数值时，url-loader 将图片进行 base64 编码(当项目中有很多图片，通过 url-loader 进行 base64 编码后会减少 http 请求数量，提高性能)，大于 limit 参数值，则使用 file-loader 拷贝图片并输出到编译目录中；

### 14 slice、substr 和 substring 有什么区别

`var test = 'hello world';`

- substr(start,len):从 start 开始，往后截取 len 个字符串
- substring(start,end)：从 min(start,end)开始截取到 max(start,end)

```js
test.substr(2, 5); // "llo w"
test.substring(2, 5) === test.substring(5, 2); // "llo"
test.slice(2, 5); // "llo"
```

### 15. react-redux connect 的原理是什么

- `connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])`
- connect 的作用是连接 React 组件与 Redux store
- connect 之所以会成功，是因为 Provider 组件
  - 在原应用组件上包裹一层，使原来整个应用成为 Provider 的子组件
  - 接收 Redux 的 store 作为 props，通过 context 对象传递给子孙组件上的 connect

> 有点答非所问，随便瞎扯一点吧，总比呆呆的说不会好

### 16.HTML5 有哪些新特性

- 新增标签
  - 结构化标签: article, aside, header, footer, figure, section, nav, main, mark
  - 多媒体标签:audio(embed), video(source)
  - 新图形标签:svg, canvas
- 废除的元素
  - basefont、big、center、font、s、strike、tt、u 用 css 代替
  - applet、bgsound、blink、marquee
  - frame、noframes,在 html5 中不支持 frame 框架，只支持 iframe 框架
- 新增的 API
  - Geolocation 地理位置
  - Drag & Drop 拖放
  - Local Storage 本地存储
  - Application Cache 应用程序缓存
- DOCTYPE 声明：`<!Doctype html>`
- 新增的 input 类型和属性
  | 类型 type | 属性 attribute |
  | --------------------- | ------------------------ |
  | color | autocomplete 、autofocus |
  | datetime 、time、date | list |
  | email 、tel 、url | placeholder |
  | month 、week | required |
  | number | pattern(regexp) |
  | range | height and width |
  | search | min and max |

### 17. CSS3 有哪些新特性

- 新增很多选择器
  - :root: 选择文档的根元素
  - E:empty: 选择没有子元素的每个 E 元素（包括文本节点)。
- 动画
  - Transition
  - Transform
  - Animation
- 渐变
  - linear-gradient(线性渐变)
  - radial-gradient(径向渐变)
- border
  - border-radius
  - box-shadow
  - border-image
- 背景
  - background-clip
  - background-origin
  - background-size
  - background-break
- 文字效果
  - word-wrap
  - text-overflow
  - text-shadow
- @font-face 特性
- flex 布局

### 18. ES6 有哪些新特性

- const let
- 解构赋值（数组，对象，字符串）
- 字符串扩展
  - 模板字符串
  - includes()：返回布尔值，表示是否找到了参数字符串
  - startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
  - endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
  - repeat()：方法返回一个新字符串，表示将原字符串重复 n 次。
- 数值的扩展
  - Number.isFinite(), Number.isNaN() // 它们与传统的全局方法 isFinite()和 isNaN()的区别在于，传统方法先调用 Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效
  - Number.parseInt(), Number.parseFloat()
  - Number.isInteger(): 判断一个数值是否为整数。
- 函数的扩展
  - 参数设置默认值
  - rest 参数
  - 箭头函数
- 数组的扩展
  - 扩展运算符 ...
  - Array.from: 用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象
  - find() 和 findIndex()
  - fill()
  - includes()
  - flat()，flatMap()
- 对象的扩展
  - 扩展运算符 ...
  - 属性的简洁表示法
  - Object.assign()方法用于对象的合并，Object.assign()方法实行的是浅拷贝，而不是深拷贝
  - Object.keys()，Object.values()，Object.entries()
- Set
- Map
- Promise
- Generator
- async
- class 继承
- module 语法

> 参考：[再来一打 Webpack 面试题](https://juejin.im/post/6844904094281236487)
> 参考：[霖呆呆的近期面试 128 题汇总(含超详细答案)](https://juejin.im/post/6844904151369908232)

### 19. 常用浏览器内核以及 css 前缀

- 内核
  - IE 内核: IE、傲游、世界之窗、百度浏览器。`-ms`
  - WebKit: safari。`-webkit`
  - Blink: Chrome、Opera。`-o`
  - 火狐内核: Firefox。`-ms`
  - Chromium: Chromium。`-ms`
- css 前缀
  - -ms-transform:rotate(7deg); // -ms-代表 IE 浏览器识别前缀
  - -moz-transform:rotate(7deg); // -moz-代表火狐浏览器识别前缀
  - -webkit-transform:rotate(7deg); // -webkit-代表谷歌和 Safari 浏览器识别前缀
  - -o-transform:rotate(7deg); // -o- 代表 Opera 浏览器识别前缀

### git-flow 有了解过吗

- 是一个别人定义好的工作流程
- 并不会为 Git 扩展任何新的功能
- 安装 git-flow，会拥有一些扩展命令
