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
  - [9.webpack 中如何处理图片的](#9webpack-中如何处理图片的)
  - [10.在移动端中怎样初始化根元素的字体大小](#10在移动端中怎样初始化根元素的字体大小)
  - [11.移动端布局](#11移动端布局)
  - [12.知道 meta 标签有把 http 换成 https 的功能吗](#12知道-meta-标签有把-http-换成-https-的功能吗)
  - [13.webpack](#13webpack)
    - [13.1 webpack 和 gulp 区别](#131-webpack-和-gulp-区别)
    - [13.2 webpack 怎么打包多页面](#132-webpack-怎么打包多页面)
  - [14.性能优化](#14性能优化)
  - [15. String](#15-string)
    - [15.1 slice、substr 和 substring 有什么区别](#151-slicesubstr-和-substring-有什么区别)
  - [16. Array](#16-array)

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
  cosnt result = MyFunction.call(obj, "Li","Cherry")
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

### 9.webpack 中如何处理图片的

在 webpack 中有两种处理图片的 loader：

file-loader：解决 CSS 等中引入图片的路径问题；(解决通过 url,import/require()等引入图片的问题)
url-loader：当图片小于设置的 limit 参数值时，url-loader 将图片进行 base64 编码(当项目中有很多图片，通过 url-loader 进行 base64 编码后会减少 http 请求数量，提高性能)，大于 limit 参数值，则使用 file-loader 拷贝图片并输出到编译目录中；

### 10.在移动端中怎样初始化根元素的字体大小

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

我一般的解决方案是结合两个库

### 11.移动端布局

- 移动端布局的方式主要使用 rem 和 flex，可以结合 rem 和媒体查询，然后不同的上视口大小下设置设置 html 的 font-size。
- 可单独制作移动端页面也可响应式 pc 端移动端共用一个页面。没有好坏，视情况而定，因势利导
- 我一般是结合两个库`postcss-pxtorem`和`lib-flexible`，前者用于转换单位，后者用于修改根节点字体大小

### 12.知道 meta 标签有把 http 换成 https 的功能吗

利用 meta 标签把 http 请求换为 https

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" />
```

### 13.webpack

#### 13.1 webpack 和 gulp 区别

- gulp 强调的是前端开发的工作流程，我们可以通过配置一系列的 task，定义 task 处理的事务
- webpack 是一个前端模块化方案，更侧重模块打包

#### 13.2 webpack 怎么打包多页面

需要在单页面的基础上改动 2 个地方

- entry 中添加每个页面对应的入口文件
- html-webpack-plugin 添加每个页面对应的 html 模板

### 14.性能优化

- 降低请求量：合并资源，减少 HTTP 请求数，minify / gzip 压缩，webP，图片 lazyLoad。
- 加快请求速度：预解析 DNS，减少域名数，并行加载，CDN 分发。
- 缓存：HTTP 协议缓存请求，离线数据缓存 localStorage。
- 渲染：JS/CSS 优化（避免使用 CSS 表达式），加载顺序（将 CSS 样式表放在顶部，把 javascript 放在底部），服务端渲染，pipeline。

### 15. String

#### 15.1 slice、substr 和 substring 有什么区别

`var test = 'hello world';`

- substr(start,len):从 start 开始，往后截取 len 个字符串
- substring(start,end)：从 min(start,end)开始截取到 max(start,end)

```js
test.substr(2, 5); // "llo w"
test.substring(2, 5) === test.substring(5, 2); // "llo"
test.slice(2, 5); // "llo"
```

### 16. Array

> 参考：[霖呆呆的近期面试 128 题汇总(含超详细答案)](https://juejin.im/post/6844904151369908232)
