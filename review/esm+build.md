<!-- TOC -->

- [1. 模块化和打包](#1-模块化和打包)
    - [1.1. 前端模块化](#11-前端模块化)
        - [1.1.1. ES6 模块与 CommonJS 模块的差异](#111-es6-模块与-commonjs-模块的差异)
    - [1.2. 打包](#12-打包)

<!-- /TOC -->
# 1. 模块化和打包

## 1.1. 前端模块化

前端模块化就是把代码分成独立的模块有利于复用和维护。不过会有模块之间相互依赖的问题，所以有了 commonJS 规范，AMD，CMD 规范等等，以及用于 js 打包（编译等处理）的工具 ES6

- Commonjs
  - 主要用于服务端编程，加载模块是同步的，不适合在浏览器环境
- AMD
  - 主要在浏览器环境中异步加载模块，可以并行加载多个模块
  - RequireJS 实现了 AMD 规范
- CMD
  - 与 AMD 规范相似，都用于浏览器编程，不同点在于：AMD 推崇依赖前置、提前执行，CMD 推崇依赖就近、延迟执行
  - seajs 实现了 CMD 规范
- ES6 模块功能主要由两个命令构成：export 和 import

### 1.1.1. ES6 模块与 CommonJS 模块的差异

- CommonJS 模块是运行时加载，ES6 模块是编译时加载模块
- CommonJS 输出是值的拷贝；ES6 Modules 输出的是值的引用

  ```js
  // CommonJS模块
  let { stat, exists, readFile } = require('fs');

  // 等同于
  let _fs = require('fs');
  let stat = _fs.stat;
  let exists = _fs.exists;
  let readfile = _fs.readfile;
  ```

  上面代码的实质是整体加载 fs 模块（即加载 fs 的所有方法），生成一个对象（\_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”。

  ```js
  // ES6模块
  import { stat, exists, readFile } from 'fs';
  ```

  上面代码的实质是从 fs 模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。

## 1.2. 打包
- Webpack
- vite