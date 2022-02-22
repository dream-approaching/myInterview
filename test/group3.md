<!-- TOC -->

- [1. Group 3](#1-group-3)
    - [1.1. HTML5 有哪些新特性](#11-html5-有哪些新特性)
    - [1.2. CSS3 有哪些新特性](#12-css3-有哪些新特性)
    - [1.3. ES6 有哪些新特性](#13-es6-有哪些新特性)
    - [1.4. 常用浏览器内核以及 css 前缀](#14-常用浏览器内核以及-css-前缀)
    - [1.5. git-flow 有了解过吗](#15-git-flow-有了解过吗)
    - [1.6. Vite 为什么快](#16-vite-为什么快)

<!-- /TOC -->
# 1. Group 3
## 1.1. HTML5 有哪些新特性

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

## 1.2. CSS3 有哪些新特性

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

## 1.3. ES6 有哪些新特性

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

## 1.4. 常用浏览器内核以及 css 前缀

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

## 1.5. git-flow 有了解过吗

- 是一个别人定义好的工作流程
- 并不会为 Git 扩展任何新的功能
- 安装 git-flow，会拥有一些扩展命令

## 1.6. Vite 为什么快

> 相较于传统的打包构建工具（如 Webpack）先打包构建再启动开发服务器，Vite 利用了浏览器对 ESM 的支持，先启动开发服务器，当代码执行到模块加载时再请求对应模块的文件

- 冷启动
    - Vite 通过在一开始将应用中的模块区分为 依赖 和 源码 两类，改进了开发服务器启动时间。
    - Vite 以 原生 ESM 方式提供源码。这实际上是让浏览器接管了打包程序的部分工作：Vite 只需要在浏览器请求源码时进行转换并按需提供源码。根据情景动态导入代码，即只在当前屏幕上实际使用时才会被处理。
    ![](https://raw.githubusercontent.com/dream-approaching/pictureMaps/master/img/20220222181730.png)
- 热更新
    - 在 Vite 中，HMR 是在原生 ESM 上执行的。
    - Vite 同时利用 HTTP 头来加速整个页面的重新加载（再次让浏览器为我们做更多事情）：源码模块的请求会根据 304 Not Modified 进行协商缓存，而依赖模块请求则会通过 Cache-Control: max-age=31536000,immutable 进行强缓存，因此一旦被缓存它们将不需要再次请求。
