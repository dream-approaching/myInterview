<!-- TOC -->

- [1. Group 2](#1-group-2)
    - [1.1. 知道 meta 标签有把 http 换成 https 的功能吗](#11-知道-meta-标签有把-http-换成-https-的功能吗)
    - [1.2. 性能优化](#12-性能优化)
        - [1.2.1. React 中怎么优化性能](#121-react-中怎么优化性能)
    - [1.3. webpack](#13-webpack)
        - [1.3.1. webpack 和 gulp 区别](#131-webpack-和-gulp-区别)
        - [1.3.2. webpack 怎么打包多页面](#132-webpack-怎么打包多页面)
        - [1.3.3. 有哪些常见的 Loader？你用过哪些 Loader？](#133-有哪些常见的-loader你用过哪些-loader)
        - [1.3.4. 有哪些常见的 Plugin？你用过哪些 Plugin？](#134-有哪些常见的-plugin你用过哪些-plugin)
        - [1.3.5. 说一说 Loader 和 Plugin 的区别?](#135-说一说-loader-和-plugin-的区别)
        - [1.3.6. Webpack 构建流程简单说一下](#136-webpack-构建流程简单说一下)
        - [1.3.7. 如何优化 Webpack 的构建速度？](#137-如何优化-webpack-的构建速度)
        - [1.3.8. 什么是 Tree shaking？如何开启](#138-什么是-tree-shaking如何开启)
        - [1.3.9. 如何实现代码分割 code splitting](#139-如何实现代码分割-code-splitting)
        - [1.3.10. webpack 中如何处理图片的](#1310-webpack-中如何处理图片的)
    - [1.4. slice、substr 和 substring 有什么区别](#14-slicesubstr-和-substring-有什么区别)
    - [1.5. react-redux connect 的原理是什么](#15-react-redux-connect-的原理是什么)

<!-- /TOC -->

# 1. Group 2
## 1.1. 知道 meta 标签有把 http 换成 https 的功能吗

利用 meta 标签把 http 请求换为 https

```html
<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests" />
```

## 1.2. 性能优化

- 降低请求量：合并资源，减少 HTTP 请求数，minify / gzip 压缩，webP，图片 lazyLoad。
- 加快请求速度：预解析 DNS，减少域名数，并行加载，CDN 分发。
- 缓存：HTTP 协议缓存请求，离线数据缓存 localStorage。
- 渲染：JS/CSS 优化（避免使用 CSS 表达式），加载顺序（将 CSS 样式表放在顶部，把 javascript 放在底部），服务端渲染，pipeline。

### 1.2.1. React 中怎么优化性能

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

## 1.3. webpack

### 1.3.1. webpack 和 gulp 区别

- gulp 强调的是前端开发的工作流程，我们可以通过配置一系列的 task，定义 task 处理的事务
- webpack 是一个前端模块化方案，更侧重模块打包

### 1.3.2. webpack 怎么打包多页面

需要在单页面的基础上改动 2 个地方

- entry 中添加每个页面对应的入口文件
- html-webpack-plugin 添加每个页面对应的 html 模板

### 1.3.3. 有哪些常见的 Loader？你用过哪些 Loader？

- `file-loader`：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
- `url-loader`：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
- `image-loader`：加载并且压缩图片文件
- `babel-loader`：把 ES6 转换成 ES5
- `json-loader` 加载 JSON 文件（默认包含）
- `css-loader`：加载 CSS，支持模块化、压缩、文件导入等特性
- `style-loader`：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
- `postcss-loader`：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀

> 说几个就好

### 1.3.4. 有哪些常见的 Plugin？你用过哪些 Plugin？

- `define-plugin`：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
- `html-webpack-plugin`：简化 HTML 文件创建 (依赖于 html-loader)
- `uglifyjs-webpack-plugin`：不支持 ES6 压缩 (Webpack4 以前)
- `terser-webpack-plugin`: 支持压缩 ES6 (Webpack4)
- `clean-webpack-plugin`: 目录清理
- `webpack-bundle-analyzer`: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)

### 1.3.5. 说一说 Loader 和 Plugin 的区别?

- Loader
  - 本质是一个函数，在该函数中对接收到的内容进行转换
  - 在 module.rules 中配置
- Plugin
  - 是插件，可以扩展 Webpack 的功能
  - 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
  - 在 plugins 中单独配置

### 1.3.6. Webpack 构建流程简单说一下

- 初始化参数
- 开始编译 => 确定入口 => 编译模块 => 完成模块编译
- 输出资源 => 输出完成

### 1.3.7. 如何优化 Webpack 的构建速度？

- 使用高版本的 Webpack 和 Node.js
- 多进程/多实例构建
- 压缩代码
- 图片压缩
- 缩小打包作用域
- 提取页面公共资源
- Tree shaking

### 1.3.8. 什么是 Tree shaking？如何开启

- Tree-shaking 是指在打包中取出那些引入了但在代码中没有被用到的死代码。
- webpack 中通过 uglifysPlugin 来 Tree-shaking JS。

### 1.3.9. 如何实现代码分割 code splitting

有三种方式

- 入口配置：entry 入口使用多个入口文件；
- 抽取公有代码：使用 SplitChunks 抽取公有代码；
- 动态加载 ：动态加载一些代码。

### 1.3.10. webpack 中如何处理图片的

在 webpack 中有两种处理图片的 loader：

file-loader：解决 CSS 等中引入图片的路径问题；(解决通过 url,import/require()等引入图片的问题)
url-loader：当图片小于设置的 limit 参数值时，url-loader 将图片进行 base64 编码(当项目中有很多图片，通过 url-loader 进行 base64 编码后会减少 http 请求数量，提高性能)，大于 limit 参数值，则使用 file-loader 拷贝图片并输出到编译目录中；

## 1.4. slice、substr 和 substring 有什么区别

`var test = 'hello world';`

- substr(start,len):从 start 开始，往后截取 len 个字符串
- substring(start,end)：从 min(start,end)开始截取到 max(start,end)

```js
test.substr(2, 5); // "llo w"
test.substring(2, 5) === test.substring(5, 2); // "llo"
test.slice(2, 5); // "llo"
```

## 1.5. react-redux connect 的原理是什么

- `connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])`
- connect 的作用是连接 React 组件与 Redux store
- connect 之所以会成功，是因为 Provider 组件
  - 在原应用组件上包裹一层，使原来整个应用成为 Provider 的子组件
  - 接收 Redux 的 store 作为 props，通过 context 对象传递给子孙组件上的 connect

> 有点答非所问，随便瞎扯一点吧，总比呆呆的说不会好
