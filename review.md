<!-- TOC -->

- [html + css](#html--css)
    - [用 border 绘制三角形](#用-border-绘制三角形)
    - [修改 placeholder 样式](#修改-placeholder-样式)
    - [自定义滚动条](#自定义滚动条)
    - [隐藏滚动条](#隐藏滚动条)
  - [超出省略号](#超出省略号)
    - [固定行数超出省略号](#固定行数超出省略号)
    - [css 盒模型](#css-盒模型)
    - [css3 动画样式](#css3-动画样式)
    - [css 变量 / css 自定义属性](#css-变量--css-自定义属性)
    - [BFC](#bfc)
    - [垂直居中](#垂直居中)
- [javascript](#javascript)
    - [冒泡/捕获](#冒泡捕获)
    - [跨浏览器的事件处理程序](#跨浏览器的事件处理程序)
    - [事件委托](#事件委托)
    - [原型链](#原型链)
    - [实现继承的几种方式](#实现继承的几种方式)
        - [优缺点对比：](#优缺点对比)
    - [new 内部的原理](#new-内部的原理)
    - [instanceof / typeof](#instanceof--typeof)
    - [this、call、apply、bind](#thiscallapplybind)
    - [作用域](#作用域)
    - [闭包](#闭包)
    - [造成内存泄漏的常见情况](#造成内存泄漏的常见情况)
    - [堆栈的区别](#堆栈的区别)
    - [ES6](#es6)
        - [var 和 const let 区别](#var-和-const-let-区别)
        - [Promise](#promise)
        - [async await](#async-await)
        - [generator](#generator)
        - [Set / Map](#set--map)
        - [Proxy](#proxy)
        - [Symbol](#symbol)
        - [Interator / for of](#interator--for-of)
        - [Object 扩展](#object-扩展)
        - [Array 扩展](#array-扩展)
    - [Array](#array)
    - [for in / for of](#for-in--for-of)
    - [事件循环(EventLoop)](#事件循环eventloop)
        - [宏任务，微任务](#宏任务微任务)
    - [浅拷贝 深拷贝](#浅拷贝-深拷贝)
    - [防抖、节流](#防抖节流)
    - [柯里化](#柯里化)
    - [常见设计模式](#常见设计模式)
- [React](#react)
    - [生命周期](#生命周期)
    - [组件之间怎么通信](#组件之间怎么通信)
    - [react 中 key 的作用是什么](#react-中-key-的作用是什么)
    - [diff 算法](#diff-算法)
    - [function Component / class Component todo](#function-component--class-component-todo)
    - [context](#context)
    - [hooks](#hooks)
    - [react hooks todo](#react-hooks-todo)
    - [redux](#redux)
      - [使用 redux 时，常用到的 api](#使用-redux-时常用到的-api)
      - [redux 数据流的走向](#redux-数据流的走向)
      - [redux 和 mobx 的区别](#redux-和-mobx-的区别)
      - [redux-saga 和 redux-thunk 对比](#redux-saga-和-redux-thunk-对比)
      - [redux-saga 常用的 api](#redux-saga-常用的-api)
      - [umi](#umi)
    - [组件的复用](#组件的复用)
    - [react 和 vue 的区别](#react-和-vue-的区别)
- [RN todo](#rn-todo)
- [浏览器](#浏览器)
    - [浏览器缓存](#浏览器缓存)
    - [浏览器的本地存储 session、cookie、localstorage、sessionStorage、IndexDB](#浏览器的本地存储-sessioncookielocalstoragesessionstorageindexdb)
    - [输入一个网址到页面展示，发生了什么事情](#输入一个网址到页面展示发生了什么事情)
    - [TCP 三次握手 四次挥手](#tcp-三次握手-四次挥手)
    - [常见状态码](#常见状态码)
    - [http 请求头里都有什么内容](#http-请求头里都有什么内容)
    - [http 和 https 的区别](#http-和-https-的区别)
    - [http 1.0、http 1.1 和 http 2.0 的区别](#http-10http-11-和-http-20-的区别)
    - [http 3.0](#http-30)
    - [跨域以及常见解决办法](#跨域以及常见解决办法)
    - [XSS 攻击](#xss-攻击)
    - [CSRF 攻击](#csrf-攻击)
- [其他](#其他)
    - [前端模块化](#前端模块化)
      - [ES6 模块与 CommonJS 模块的差异](#es6-模块与-commonjs-模块的差异)
    - [前端工程化](#前端工程化)
    - [重绘和回流（重绘和重排）](#重绘和回流重绘和重排)

<!-- /TOC -->

## html + css

#### 用 border 绘制三角形

```css
width: 0;
height: 0;
border: 30px solid;
border-color: transparent transparent lightblue;
```

> 参考： [简书——CSS 绘制三角形](https://www.jianshu.com/p/9a463d50e441)

#### 修改 placeholder 样式

```css
.input::-webkit-input-placeholder {
  color: red;
}
.input:-moz-placeholder {
  color: red;
}
.input:-ms-input-placeholder {
  color: red;
}
```

#### 自定义滚动条

```css
//滚动条整体部分 定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  background-color: transparent;
}
//滚动条两端的按钮
::-webkit-scrollbar-button {
  background-color: transparent;
}
// 外层轨道
::-webkit-scrollbar-track {
  border-radius: 10px;
  background-color: transparent;
}
//内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-track-piece {
  background-color: transparent;
}
//滚动条里面可以拖动的那个
::-webkit-scrollbar-thumb {
  border-radius: 3px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  background-color: #294269;
}
//边角
::-webkit-scrollbar-corner {
  background-color: transparent;
}
//定义右下角拖动块的样式
::-webkit-resizer {
  background-color: transparent;
}
```

#### 隐藏滚动条

```css
::-webkit-scrollbar {
  width: 0px;
}

::-webkit-scrollbar-track {
  background-color: none;
}

::-webkit-scrollbar-thumb {
  background-color: none;
}

::-webkit-scrollbar-thumb:hover {
  background-color: none;
}

::-webkit-scrollbar-thumb:active {
  background-color: none;
}
```

### 超出省略号

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
width: 160px;
```

#### 固定行数超出省略号

```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
/* autoprefixer: off */
-webkit-box-orient: vertical;
/* autoprefixer: on */
```

autoprefixer 不仅会帮你加-webkit-之类的 prefixer，它还会帮你删除你自己写在 css/sass/less 里的样式

> 参考: [-webkit-box-orient 不见了， webkit 和 autoprefixer 的坑](https://blog.csdn.net/sinat_24070543/article/details/79755285)

#### css 盒模型

- box-sizing: content-box（W3C 盒模型，又名标准盒模型）：元素的宽高大小表现为内容的大小。
- box-sizing: border-box（IE 盒模型，又名怪异盒模型）：元素的宽高表现为内容 + 内边距 + 边框的大小。背景会延伸到边框的外沿。

> 若不声明 DOCTYPE 类型，IE 浏览器会将盒子模型解释为 IE 盒子模型，FireFox 等会将其解释为 W3C 盒子模型；若在页面中声明了 DOCTYPE 类型，所有的浏览器都会把盒模型解释为 W3C 盒模型。  
> html5 中写`<!DOCTYPE html>`

#### css3 动画样式

- transform
  ```css
  transform: rotate(10deg); /* 旋转 */
  transform: skew(20deg); /* 倾斜 */
  transform: scale(1.5); /* 缩放 */
  transform: translate(120px, 0); /* 位移 */
  ```
- transition：允许 css 的属性值在一定的时间区间内平滑地过渡,有如些 4 个参数
  - `transition-property`：all / none / indent(属性名)
  - `transition-duration`：持续时间
  - `transition-timing-function`：缓动函数(linear、ease、ease-in、ease-out、ease-in-out)
  - `transition-delay`：延迟
- animation
  ```css
  /* animation: 动画名称 时长必填 缓动函数 延时 执行次数 动画方向 */
  div {
    animation: myfirst 5s linear 2s infinite alternate;
  }
  /* 也可用from to 等同于 0% 100% */
  @keyframes myfirst {
    0% {
      background: red;
    }
    25% {
      background: yellow;
    }
    50% {
      background: blue;
    }
    100% {
      background: green;
    }
  }
  ```

#### css 变量 / css 自定义属性

```css
/* 通过 -- 定义css属性  */
.foo {
  color: red;
  --theme-color: gray;
}

/* 使用css属性 一个变量可用于多个地方 */
.button {
  background-color: var(--theme-color);
}
.title {
  color: var(--theme-color);
}
.image-grid > .image {
  border-color: var(--theme-color);
}

/* 加上默认值 */
.button {
  background-color: var(--theme-color, gray);
}
/* 如果默认值也是css属性 */
.button {
  background-color: var(--theme-color, var(--fallback-color));
}

/* 可通过使用 :root 伪元素将css属性设置为全局变量，处处可用 */
:root {
  --theme-color: gray;
}
```

- 自定义元素的定义由 `--` 开头，这样浏览器能够区分自定义属性和原生属性
- 使用时用`var()`
- 可通过`:root`定义全局变量，局部想覆盖可重新定义
- 可与 calc 进行计算`calc(var(--title-multiplier) * var(--base-size))`
- 和 js 交互
  - 可以通过 `getPropertyValue` 和 `setProperty` 方法操作
    ```js
    const styles = getComputedStyle(document.querySelector('.foo'));
    // Read value. Be sure to trim to remove whitespace.
    const oldColor = styles.getPropertyValue('--color').trim();
    // Write value.
    foo.style.setProperty('--color', 'green');
    ```

#### BFC

> 常见于 margin 发生折叠情况

- BFC 即 Block Formatting Contexts (块级格式化上下文)
- 只要元素满足下面任一条件即可触发 BFC 特性
  - body 根元素
  - 浮动元素：float 除 none 以外的值
  - 绝对定位元素：position (absolute、fixed)
  - display 为 inline-block、table-cells、flex
  - overflow 除了 visible 以外的值 (hidden、auto、scroll)
- BFC 特性及应用
  - 同一个 BFC 中外边距会发生折叠(如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中)
  - BFC 可以包含浮动的元素（清除浮动）
  - BFC 可以阻止元素被浮动元素覆盖

#### 垂直居中

- line-height 等于 hieght
- vertical-align: middle 需要是 inline-block
- 绝对定位 `top:50%` 配合 `margin-top: -height / 2` 或者 `transform:translateY(-50%);`(优点是不需要知道宽高)
- 绝对定位 top、right、bottom、left 都为 0 配合 margin: auto;
- flex 布局

## javascript

#### 冒泡/捕获

- DOM2 级事件规定的事件流包括三个阶段:
  - 事件捕获阶段
  - 处于目标阶段
  - 事件冒泡阶段</br>
    ![](https://user-images.githubusercontent.com/25027560/38007715-4cc457d0-327d-11e8-9fb3-667fa75fc38c.png)
- 阻止冒泡
  ```js
  function stopPropagation(event){
    if(event.stopPropagation){
       event.stopPropagation();
    }else{
       event.cancelBubble=true;
    }
  },
  ```

#### 跨浏览器的事件处理程序

- 实现

  ```js
    var EventUtil = {
      addHandler: (element, type, handler) => {},

      removeHandler: (element, type, handler) => {}，
      // 获取event对象
      getEvent: (event) => {
        return event ? event : window.event
      },
      // 获取当前目标
      getTarget: (event) => {
        return event.target ? event.target : event.srcElement
      },
      // 阻止默认行为
      preventDefault: (event) => {
        if (event.preventDefault) {
          event.preventDefault()
        } else {
          event.returnValue = false
        }
      },
      // 停止传播事件
      stopPropagation: (event) => {
        if (event,stopPropagation) {
          event.stopPropagation()
        } else {
          event.cancelBubble = true
        }
      }
    }
  ```

> - 参考：[博客园——跨浏览器的事件对象](https://www.cnblogs.com/hykun/p/EventUtil.html)

#### 事件委托

- 用来解决事件处理程序过多的问题
- 使用场景
  ```js
    // 假设有如下页面结构
    <ul id="myLinks">
      <li id="goSomewhere">Go somewhere</li>
      <li id="doSomething">Do something</li>
      <li id="sayHi">Say hi</li>
    </ul>
    // 传统的做法，是分别为li添加 3 个事件
    // 因为子节点的点击事件会冒泡到父节点,只需在 DOM树中尽量最高的层次上添加一个事件处理程序，这种方式叫事件委托
    var list = document.getElementById("myLinks");
    EventUtil.addHandler(list, "click", function(event) {
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
      switch(target.id) {
      case "doSomething":
          document.title = "I changed the document's title";
          break;
      case "goSomewhere":
          location.href = "http://www.wrox.com";
          break;
      case "sayHi": 9 alert("hi");
        break;
      }
    }
  ```

> 参考：[github——捕获与冒泡、事件处理程序、事件对象、跨浏览器、事件委托](https://github.com/amandakelake/blog/issues/38)

#### 原型链

原型链是一种机制，指的是 JavaScript 每个对象(包括原型对象)，都有一个内置的[[proto]]属性指向创建它的函数对象的原型对象，即 prototype 属性。

原型链的存在，主要是为了实现对象的继承。

访问对象的属性时，JavaScript 会首先在对象自身的属性内查找，若没有找到，则会跳转到该对象的原型对象中查找。

1.  实例的**proto** === 其构造函数的 prototype
    ![](https://note.youdao.com/yws/public/resource/5decefaed3b17cd4bab92965ace4d207/xmlnote/36A4E7356D3A42A6A8655EEEF65CEEB2/4117)

    ```js
    function Person(name) {
      this.name = name;
    }
    let p = new Person('longzi');
    console.log(p.__proto__); // Person.prototype
    console.log(Person.__proto__); // Function.prototype
    ```

2.  `Function.__proto__ === Function.prototype`</br>
    ![](https://note.youdao.com/yws/public/resource/5decefaed3b17cd4bab92965ace4d207/xmlnote/E02A3DBFB32641C7A10EBC8982CCB875/4114)
3.  原型链的最顶层都是 null

> - 参考：这篇写的很好 多看几遍：[知乎——JavaScript 世界万物诞生记](https://zhuanlan.zhihu.com/p/22989691)
> - 参考：这篇也可以 作为上面的补充：[CSDN——prototype、**proto**与 constructor](https://blog.csdn.net/cc18868876837/article/details/81211729)

#### 实现继承的几种方式

```js
// 假设有父类 Parent
function Parent(name, age) {
  this.name = name;
  this.age = age;
  this.colors = ['red', 'blue', 'yellow'];
}
Parent.prototype.showName = function () {
  console.log(this.name);
};
```

- 原型链继承
  ```js
  function Child() {
    this.type = 'child';
  }
  Child.prototype = new Parent();
  var a = new Child();
  var b = new Child();
  console.log(a.showName()); // undefined
  console.log(a.colors); // ["red","blue","yellow"]
  console.log(b.colors); // ["red","blue","yellow"]
  a.colors.push('black');
  console.log(a.colors); // ["red","blue","yellow","black"]
  console.log(b.colors); // ["red","blue","yellow","black"]
  ```
- 构造函数继承
  ```js
  function Child(name, age) {
    Parent.call(this, name, age); // 或apply
  }
  var child1 = new Child('longzi', 23);
  var child2 = new Child('xiaofeng', 18);
  child1.colors.push('black');
  console.log(child1.colors); // ["red", "blue", "yellow", "black"]
  console.log(child2.colors); //["red", "blue", "yellow"]
  console.log(child1.name, child1.age); // longzi, 23
  console.log(child2.name, child2.age); // xiaofeng, 18
  console.log(child1.showName()); // Uncaught TypeError: child1.showName is not a function
  ```
- 组合继承(原型链+构造函数)
  ```js
  function Child(name, age) {
    Parent.call(this, name, age); // 构造函数继承
  }
  Child.prototype = new Parent();
  var child1 = new Child('longzi', 23);
  var child2 = new Child('xiaofeng', 18);
  child1.colors.push('black');
  console.log(child1.colors); // ["red", "blue", "yellow", "black"]
  console.log(child2.colors); //["red", "blue", "yellow"]
  console.log(child1.showName()); // longzi
  ```
- Object.create()
  ```js
  function Child(name, age) {
    Parent.call(this, name, age); // 构造函数继承
  }
  Child.prototype = Object.create(Parent.prototype);
  Child.prototype.constructor = Child; // 这步不要忘了
  var child1 = new Child('longzi', 23);
  var child2 = new Child('xiaofeng', 18);
  child1.colors.push('black');
  console.log(child1.colors); // ["red", "blue", "yellow", "black"]
  console.log(child2.colors); //["red", "blue", "yellow"]
  console.log(child1.showName()); // longzi
  ```
- ES6: extends
  ```js
  class Parent {}
  class Child1 extends Parent {
    constructor(x, y, colors) {
      super(x, y); // 调用父类的constructor(x, y)
      this.colors = colors;
    }
    toString() {
      return this.colors + ' ' + super.toString(); // 调用父类的toString()
    }
  }
  ```

###### 优缺点对比：

| ---             | 优点                                                                     | 缺点                                                                                   |
| --------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| 原型链继承      | 可以继承原型链上的属性和方法，查找效率高                                 | 1.父类实例属性为引用类型时，不恰当地修改会导致所有子类被修改</br> 2.无法给实例传递参数 |
| 构造函数继承    | 1.可以在 Child 中向 Parent 传参</br>2.避免了引用类型的属性被所有实例共享 | 父类原型上的东西是没法继承的                                                           |
| 组合继承        | 解决上述两种方式的缺点                                                   | 调用了两次父类的构造函数                                                               |
| Object.create() | 1.解决上述三种方式的缺点</br>2.ES5 首选                                  | 暂无                                                                                   |
| class.extends   | 清晰 方便                                                                | 注意：子类必须在 constructor 方法中调用 super 方法，否则新建实例时会报错。             |

> - 参考：[简书——Javascript 的继承与多态](https://www.jianshu.com/p/5cb692658704)
> - 参考：[Dunizb——JS 继承方式总结](https://mp.weixin.qq.com/s?__biz=MzI0MDIwNTQ1Mg==&mid=2676491918&idx=1&sn=2a30b02356595e974537c78b2a82f8eb&chksm=f362cd6dc415447b96cd97db4857b40146ee1b8ba1191ecffdb30644152c6386373a2b5717c1#rd)
> - 参考：[TG——JavaScript 实现继承的方式](http://ghmagical.com/article/page/id/omIoPb1AIBPu)
> - 参考：[ECMAScript 6 入门——class 的继承](http://es6.ruanyifeng.com/#docs/class-extends)

#### new 内部的原理

```js
var a = new myFunction("Li","Cherry");
new myFunction{
    var obj = {}; // 创建一个空对象 obj
    obj.__proto__ = myFunction.prototype; // 将新创建的空对象的隐式原型指向其构造函数的显示原型
    var result = myFunction.call(obj,"Li","Cherry"); // 使用 call 改变 this 的指向
    return typeof result === 'object'? result : obj; // 如果无返回值或者返回一个非对象值，则将 obj 返回作为新对象；如果返回值是一个新对象的话那么直接直接返回该对象
}
```

#### instanceof / typeof

- 例子
  ```js
  let s = new String('abc');
  typeof s; // object
  s instanceof String; // true
  typeof null; // object
  null instanceof null; // TypeError: Right-hand side of 'instanceof' is not an object
  function Foo() {}
  Foo instanceof Object; // true
  Foo instanceof Function; // true
  ```
- typeof  
   typeof 一般用来判断 number, string, object, boolean, function, undefined, symbol
- instanceof
  - 主要是用于实例的判断。 `A instanceof B` 用来判断 A 是否为 B 的实例
  - 也可以判断一个实例是否是其父类型或者祖先类型的实例
    ```js
    const person = function () {};
    const programmer = function () {};
    programmer.prototype = new person();
    let nicole = new programmer();
    nicole instanceof person; // true
    nicole instanceof programmer; // true
    ```

#### this、call、apply、bind

- 下面的文章写得好，多看几遍，懒得总结了
- 参考：[掘金——this、apply、call、bind](https://juejin.im/post/59bfe84351882531b730bac2)
- 参考：[掘金——this（他喵的）到底是什么 — 理解 JavaScript 中的 this、call、apply 和 bind](https://juejin.im/post/5b9f176b6fb9a05d3827d03f)

#### 作用域

- 作用域链  
   在查找变量的时候，先在函数作用域中查找，没有找到，再去全局作用域中查找，有一个从里往外查找的过程。
  ![image](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/F1866E20E2F34E769495F1BEF73D6803/4723)
- 词法作用域(静态作用域)  
   JavaScript 采用的是词法作用域，函数的作用域在函数定义的时候就决定了，即在书写代码时就确定了
  ```js
  var value = 1;
  function foo() {
    console.log(value);
  }
  function bar() {
    var value = 2;
    foo();
  }
  bar(); // 1
  // 执行 foo 函数时，先从 foo 函数内部查找是否有局部变量 value，如果没有，就根据书写的位置，查找上面一层的代码，也就是 value 等于 1，所以结果会打印 1。
  ```
- 动态作用域  
   动态作用域与词法作用域相反，函数的作用域是在函数调用的时候才决定的。  
   上面代码如果是动态作用域，则会输出 2

#### 闭包

- 概念：闭包是一个可以访问它外部函数作用域的一个函数，即使这个外部函数已经返回了。
- 闭包是一种现象，不用特意创建，有时候不知不觉就写出来了
- 函数的作用域及其所有变量都会在函数执行结束后被销毁。但是，在创建了一个闭包以后，这个函数的作用域就会一直保存到闭包不存在为止。
- 缺陷：缺点就是常驻内存会增大内存使用量，在老的 IE 中很容易造成内存泄露。
- 运用闭包的关键：
  - 闭包引用外部函数变量对象中的值
  - 在外部函数的外部调用闭包。
- 例子 1：
  ```js
  function person() {
    let name = 'Peter';
    return function displayName() {
      console.log(name);
    };
  }
  let peter = person();
  peter(); // prints 'Peter'。
  // 在displayName函数中并没有定义任何名为name到变量，所以即使该函数返回了，该函数也可以用某种方式访问其外部函数person的变量。
  // 所以displayName函数实际上是一个闭包。
  ```
- 例子 2：
  ```js
  function getCounter() {
    let counter = 0;
    return function () {
      return counter++;
    };
  }
  let count = getCounter();
  console.log(count()); // 0
  console.log(count()); // 1
  console.log(count()); // 2
  // 调用getCounter函数返回一个匿名内部函数，保存到count变量中。此时count函数现在是一个闭包，可以在即使在getCounter函数返回后访问getCounter函数的变量couneter。
  // 但是请注意，counter的值在每次count函数调用时都不会像通常那样重置为0。因为每次调用count()的时候，都会创建新的函数作用域，但是只为getCounter函数创建一个作用域，因为变量counter定义在getCounter函数作用域内，所以每次调用count函数时数值会增加而不是重置为0。
  ```
- 常见闭包面试题

  ```js
  for (var i = 1; i < 10; i++) {
    setTimeout(function () {
      console.log(i);
    }, 1000);
  }
  // 10 10 10 10 10 10 10 10 10 10

  for (let i = 1; i < 10; i++) {
    setTimeout(function () {
      console.log(i);
    }, 1000);
  }
  // 1 2 3 4 5 6 7 8 9

  for (var i = 1; i <= 10; i++) {
    (function (j) {
      setTimeout(function () {
        console.log(j);
      }, 1000);
    })(i);
  }
  // 1 2 3 4 5 6 7 8 9
  ```

> 参考：[掘金——闭包详解](https://juejin.im/post/5b081f8d6fb9a07a9b3664b6)  
> 参考：[掘金——理解 JavaScript 闭包——新手指南](https://juejin.im/post/6844903726205911053)

#### 造成内存泄漏的常见情况

- 意外的全局变量
- 定时器未清除
- dom 清空时，还存在引用
- 闭包(只有老 ie 会)

```js
var a = { a: 1, b: 2, c: 3, d: 4 };
for (let v of a) {
  console.log(v); // red green blue
}
```

#### 堆栈的区别

- 栈(stack)
  - 会自动分配内存空间，会自动释放。
  - 存放简单的数据段，占据固定大小的空间。
  - 属于后进先出
    ```js
    function multiply(x, y) {
      return x * y;
    }
    function printSquare(x) {
      var s = multiply(x, x);
      console.log(s);
    }
    printSquare(5);
    ```
    ![](https://user-gold-cdn.xitu.io/2017/11/11/bc37a6231fca3b0aa3cd36369e866837?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
- 堆(heap)
  - 动态分配的内存，大小不定也不会自动释放。
  - 存放引用类型变量的指针

> 参考：[[译] JavaScript 如何工作：对引擎、运行时、调用堆栈的概述](https://juejin.im/post/6844903510538993671)

#### ES6

###### var 和 const let 区别

- 块级作用域
- 不存在变量提升
- 暂时性死区
- 不可重复声明
- let、const 声明的  全局变量不会挂在顶层对象下面
- const 声明之后必须马上赋值，否则会报错
- const 简单类型一旦声明就不能再更改， 复杂类型(数组、对象等)指针指向的地址不能更改，内部数据可以更改

###### Promise

Promise 是异步编程的一种解决方案, 有以下两个特点:

- 对象的状态不受外界影响
- 一旦状态改变，就不会再变

**常用**

- resolve 函数在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
- reject 函数，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
- Promise.prototype.finally()：用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。
  ```js
  promise
  .then(result => {···})
  .catch(error => {···})
  .finally(() => {···});
  ```
- Promise.all()
  - 用于将多个 Promise 实例，包装成一个新的 Promise 实例
  - 接受一个数组作为参数，p1、p2、p3 都是 Promise 实例，如果不是，就会先调用 Promise.resolve 方法，将参数转为 Promise 实例
  - Promise.all 方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
    ```js
    const p = Promise.all([p1, p2, p3]);
    // 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
    // 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
    ```
- Promise.race()
  - 基本语法与 Promise.all()类似
  - 返回的规则不同
    ```js
    const p = Promise.race([p1, p2, p3]);
    // 只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。
    // 率先改变的 Promise 实例的返回值，就传递给p的回调函数
    ```
- Promise.resolve(): 将现有对象转为 Promise 对象
- Promise.reject(reason): 返回一个新的 Promise 实例，该实例的状态为 rejected

  ```js
  const p = Promise.reject('出错了');
  // 等同于
  const p = new Promise((resolve, reject) => reject('出错了'));

  p.then(null, function (s) {
    console.log(s);
  });
  // 出错了
  ```

###### async await

- 是 Generator 函数的语法糖
- 与 Generator 的区别
  - 内置执行器：Generator 函数的执行必须靠执行器，async 函数的执行，与普通函数一模一样，只要一行
  - 更好的语义
  - Generator 函数的返回值是 Iterator 对象，async 函数的返回值是 Promise 对象
- 遇到 await 表达式时，会让 async 函数 暂停执行，等到 await 后面的语句（Promise）状态发生改变（resolved 或者 rejected）之后，再恢复 async 函数的执行（再之后 await 下面的语句），并返回解析值（Promise 的值）
- 例子

  ```js
  async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
  }
  async function async2() {
    console.log('async2');
  }
  console.log('script start');
  setTimeout(function () {
    console.log('setTimeout');
  }, 0);
  async1();
  new Promise(function (resolve) {
    console.log('promise1');
    resolve();
  }).then(function () {
    console.log('promise2');
  });
  console.log('script end');

  // script start
  // async1 start
  // async2
  // promise1
  // script end
  // async1 end
  // promise2
  // setTimeout
  ```

> 参考：[掘金——前端 er，你真的会用 async 吗？](https://juejin.im/post/5c0397186fb9a049b5068e54)

###### generator

- Generator 是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
- 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，必须调用遍历器对象的 next 方法，使得指针移向下一个状态
- 换言之，Generator 函数是分段执行的，yield 表达式是暂停执行的标记，而 next 方法可以恢复执行

  ```js
  function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
  }

  var hw = helloWorldGenerator();
  hw.next(); // { value: 'hello', done: false }
  hw.next(); // { value: 'world', done: false }
  hw.next(); // { value: 'ending', done: true }
  hw.next(); // { value: undefined, done: true }
  ```

- Generator 函数可以不用 yield 表达式，这时就变成了一个单纯的暂缓执行函数。
- 语法：下面四种都行
  ```js
  function * foo(x, y) { ··· }
  function *foo(x, y) { ··· }
  function* foo(x, y) { ··· }
  function*foo(x, y) { ··· }
  ```
- yield 表达式
  - 遇到 yield 表达式，就暂停执行后面的操作，并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值
  - 下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。
  - 如果没有再遇到新的 yield 表达式，就一直运行到函数结束，直到 return 语句为止，并将 return 语句后面的表达式的值，作为返回的对象的 value 属性值。
  - 如果该函数没有 return 语句，则返回的对象的 value 属性值为 undefined。
- yield\*表达式

  - 在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的
  - yield\*表达式，用来在一个 Generator 函数里面执行另一个 Generator 函数

    ```js
    function* bar() {
      yield 'x';
      yield* foo();
      yield 'y';
    }

    // 等同于
    function* bar() {
      yield 'x';
      yield 'a';
      yield 'b';
      yield 'y';
    }

    // 等同于
    function* bar() {
      yield 'x';
      for (let v of foo()) {
        yield v;
      }
      yield 'y';
    }

    for (let v of bar()) {
      console.log(v);
    }
    // "x"
    // "a"
    // "b"
    // "y"
    ```

- for...of 循环

  - for...of 循环可以自动遍历 Generator 函数运行时生成的 Iterator 对象，且此时不再需要调用 next 方法。

    ```js
    function* foo() {
      yield 1;
      yield 2;
      yield 3;
      yield 4;
      yield 5;
      return 6;
    }

    for (let v of foo()) {
      console.log(v);
    }
    // 1 2 3 4 5
    // 上面代码使用for...of循环，依次显示 5 个yield表达式的值。
    // 这里需要注意，一旦next方法的返回对象的done属性为true，for...of循环就会中止，且不包含该返回对象
    // 所以上面代码的return语句返回的6，不包括在for...of循环之中
    ```

###### Set / Map

- Set

  - 类似于数组，但是成员的值都是唯一的，没有重复的值
  - Set 加入值的时候，不会发生类型转换，5 和"5"是两个不同的值
  - Set 内部, 两个 NaN 是相等
  - Set 本身是一个构造函数，用来生成 Set 数据结构。
  - Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数

    ```js
    const s = new Set();
    [2, 3, 5, 4, 5, 2, 2].forEach((x) => s.add(x));
    for (let i of s) {
      console.log(i);
    } // 2 3 5 4

    // 字符串去重
    [...new Set('ababbc')].join(''); //"abc"
    ```

  - Array.from 方法可以将 Set 结构转为数组。
    ```js
    const items = new Set([1, 2, 3, 4, 5]);
    const array = Array.from(items);
    console.log(array); // [1, 2, 3, 4, 5]
    ```
  - 使用 Set 可以很容易地实现并集、交集和差集

    ```js
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);

    // 并集
    let union = new Set([...a, ...b]);
    // Set {1, 2, 3, 4}

    // 交集
    let intersect = new Set([...a].filter((x) => b.has(x)));
    // set {2, 3}

    // 差集
    let difference = new Set([...a].filter((x) => !b.has(x)));
    // Set {1}
    ```

  - 实例属性：Set.prototype.size：返回 Set 实例的成员总数
  - 实例方法
    - add(value)：添加某个值，返回 Set 结构本身。
    - delete(value)：删除某个值，返回一个布尔值
    - has(value)：返回一个布尔值，表示该值是否为 Set 的成员。
    - clear()：清除所有成员，没有返回值。

- WeakSet: (尽量记一下)
  - WeakSet 的成员只能是对象，而不能是其他类型的值
  - WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中
- Map

  - 它类似于对象，也是键值对的集合
  - 但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键
  - 实例属性：size：属性返回 Map 结构的成员总数。
  - 实例方法

    - set(key, value)：set 方法设置键名 key 对应的键值为 value，然后返回整个 Map 结构

      ```js
      const m = new Map();

      m.set('edition', 6); // 键是字符串
      m.set(262, 'standard'); // 键是数值
      m.set(undefined, 'nah'); // 键是 undefined
      ```

    - get(key):读取 key 对应的键值，如果找不到 key，返回 undefined。
    - delete(key):删除某个键，返回一个布尔值
    - has(value)：返回一个布尔值，表示某个键是否在当前 Map 对象之中。
    - clear()：清除所有成员，没有返回值。

- WeakMap
  - WeakMap 只接受对象作为键名（null 除外），不接受其他类型的值作为键名
  - WeakMap 的键名所指向的对象，不计入垃圾回收机制。

###### Proxy

- 在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截
- ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例
  `var proxy = new Proxy(target, handler);`
  > 能记这些就差不多了，再问直接放弃

###### Symbol

- 一种新的原始数据类型 Symbol，表示独一无二的值
- Symbol 函数前不能使用 new 命令，否则会报错
- Symbol 函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

  ```js
  let s = Symbol();
  typeof s; // "symbol"

  let s1 = Symbol('foo');
  let s2 = Symbol('bar');
  s1; // Symbol(foo)
  s2; // Symbol(bar)
  s1.toString(); // "Symbol(foo)"
  s2.toString(); // "Symbol(bar)"
  ```

  > 先记这么多，脑子不够用了

###### Interator / for of

- 原生具备 Iterator 接口的数据结构如下。
  - Array
  - Map
  - Set
  - String
  - TypedArray
  - 函数的 arguments 对象
  - NodeList 对象
- for...of
  - for...of 循环内部调用的是数据结构的 Symbol.iterator 方法
  - 一个数据结构只要部署了 Symbol.iterator 属性，就被视为具有 iterator 接口，就可以用 for...of 循环遍历它的成员。
- 其他调用 Iterator 接口的场合
  - 解构赋值：对数组和 Set 结构进行解构赋值时
  - 扩展运算符（...）
  - Array.from()
  - yield\*
  - Promise.all() / Promise.race()

###### Object 扩展

- Object.assign
  - 第一个参数是目标对象，后面的参数都是源对象，返回目标对象
  - 如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
  - 只拷贝源对象的自身属性（不拷贝继承属性）
  - 也不拷贝不可枚举的属性（enumerable: false）
- Object.setPrototypeOf()
  - 作用与**proto**相同，用来设置一个对象的 prototype 对象，返回参数对象本身
  - 语法：Object.setPrototypeOf(object, prototype)
  - 等同于
    ```js
    function setPrototypeOf(obj, proto) {
      obj.__proto__ = proto;
      return obj;
    }
    ```
- Object.getPrototypeOf()
  - 与 Object.setPrototypeOf 方法配套，用于读取一个对象的原型对象
  - 语法：Object.getPrototypeOf(obj);
- Object.keys()
  - ES5 引入
  - 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。
  - 例子：
    ```js
    var obj = { foo: 'bar', baz: 42 };
    Object.keys(obj);
    // ["foo", "baz"]
    ```
- Object.values()
  - 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。
  - 只返回对象自身的可遍历属性
  - 例子：
    ```js
    const obj = { foo: 'bar', baz: 42 };
    Object.values(obj);
    // ["bar", 42]
    ```
- Object.entries()
  - 方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组
  - 例子
    ```js
    const obj = { foo: 'bar', baz: 42 };
    Object.entries(obj);
    // [ ["foo", "bar"], ["baz", 42] ]
    ```

###### Array 扩展

- Array.from()
  - 只要是部署了 Iterator 接口的数据结构，Array.from 都能将其转为数组
  - Array.from 还可以接受第二个参数，作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
- Array.of()
  - 用于将一组值，转换为数组
  - 这个方法的主要目的，是弥补数组构造函数 Array()的不足。因为参数个数的不同，会导致 Array()的行为有差异。
    ```js
    Array(); // []
    Array(3); // [, , ,]
    Array(3, 11, 8); // [3, 11, 8]
    ```
  - 例子：
    ```js
    Array.of(3, 11, 8); // [3,11,8]
    Array.of(3); // [3]
    Array.of(3).length; // 1
    ```
- 数组实例的 find() 和 findIndex()
  - find 方法，用于找出第一个符合条件的数组成员。
  - 参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为 true 的成员，然后返回该成员。
  - 如果没有符合条件的成员，则返回 undefined。
    ```js
    [1, 5, 10, 15].find(function (value, index, arr) {
      return value > 9;
    }); // 10
    ```
  - findIndex 方法的用法与 find 方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。
- 数组实例的 fill()
  - 主要用于空数组的初始化非常方便
    ```js
    new Array(3).fill(7); // [7, 7, 7]
    ```
- 数组实例的 includes()
  - 返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。
    ```js
    [1, 2, 3]
      .includes(2) // true
      [(1, 2, 3)].includes(4) // false
      [(1, 2, NaN)].includes(NaN); // true
    ```
  - Map 和 Set 数据结构有一个 has 方法, 需要注意与 includes 区分
    - Map 结构的 has 方法，是用来查找键名的
    - Set 结构的 has 方法，是用来查找值的
- 数组实例的 flat()，flatMap()
  - 用于将嵌套的数组“拉平”，变成一维的数组。
  - 返回一个新数组，对原数据没有影响。
  - 默认只会“拉平”一层
    ```js
    [1, 2, [3, [4, 5]]]
      .flat() // [1, 2, 3, [4, 5]]
      [(1, 2, [3, [4, 5]])].flat(2) // [1, 2, 3, 4, 5]
      [(1, [2, [3]])].flat(Infinity); // [1, 2, 3]
    ```
  - flatMap()方法对原数组的每个成员执行一个函数（相当于执行 map()），然后对返回值组成的数组执行 flat()方法
  - flatMap()只能展开一层数组
    ```js
    // 相当于 [[2, 4], [3, 6], [4, 8]].flat()
    [2, 3, 4].flatMap((x) => [x, x * 2]);
    // [2, 4, 3, 6, 4, 8]
    ```

#### Array

- forEach map reduce filter some every
  ![](https://raw.githubusercontent.com/dream-approaching/pictureMaps/master/img/arr.png)

  > 参考 [掘金——数组迭代方法图解](https://juejin.im/post/5835808067f3560065ed4ab2)

- reduce:

  - 语法 `arr.reduce(callback,[initialValue])`
    - callback （执行数组中每个值的函数，包含四个参数）
      - previousValue （上一次调用回调返回的值，或者是提供的初始值（initialValue））
      - currentValue （数组中当前被处理的元素）
      - index （当前元素在数组中的索引）
      - array （调用 reduce 的数组）
    - initialValue （作为第一次调用 callback 的第一个参数。）
  - 举例

    ```js
    var arr = [
      { id: 1, type: 'A', total: 3 },
      { id: 2, type: 'B', total: 5 },
      { id: 3, type: 'E', total: 7 },
    ];
    // 统计 total 的总和
    arr.reduce((sum, { total }) => {
      return sum + total;
    }, 0); // 15

    // 转换成对象
    arr.reduce((res, { id, type, total }) => {
      res[id] = { type, total };
      return res;
    }, {});
    // {1:{type: 'A', total: 3}, 2: { type: 'B', total: 5 },{ type: 'E', total: 7 }}
    ```

- slice splice
  - slice
    - 可操控 Array 及 String
    - 返回新数组，不该变原值
    - `arr.slice(begin)` / `arr.slice(begin, end)`
  - splice
    - 可操控 Array
    - 从 Array 中添加/刪除项目，返回被刪除的项目。会改变原值
    - `array.splice(start, deleteCount, item1, item2, ...)`
      - start 增加/刪除項目的位置，負數代表從後方算起。
      - deleteCount 刪除的個數，如為 0 則不會刪除。
      - item… 添加的新項目。

#### for in / for of

- 推荐在循环对象属性的时候，使用 for...in,在遍历数组的时候的时候使用 for...of。
- for...in 循环出的是 key，for...of 循环出的是 value
- 注意，for...of 是 ES6 新引入的特性。修复了 ES5 引入的 for...in 的不足
- for...of 不能循环普通的对象，需要通过和 Object.keys()搭配使用

> 参考 [javascript 中 for of 和 for in 的区别？](https://segmentfault.com/q/1010000006658882)

#### 事件循环(EventLoop)

![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/CDE1E05AA6924122A3BC9793CF2C6D0E/5042)

- 同步和异步任务分别进入不同的执行"场所"
- 同步的进入主线程，异步的进入 Event Table 并注册函数。
- 当指定的事情完成时，Event Table 会将这个函数移入 Event Queue。
- 主线程内的任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的 Event Loop(事件循环)。

###### 宏任务，微任务

![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/AC64F1B26FF04EB5A7724F4A8E2080F9/5052)

- 例子

  ```js
  console.log('1');

  setTimeout(function () {
    console.log('2');
    process.nextTick(function () {
      console.log('3');
    });
    new Promise(function (resolve) {
      console.log('4');
      resolve();
    }).then(function () {
      console.log('5');
    });
  }, 500);
  process.nextTick(function () {
    console.log('6');
  });
  new Promise(function (resolve) {
    console.log('7');
    resolve();
  }).then(function () {
    console.log('8');
  });

  setTimeout(function () {
    console.log('9');
    process.nextTick(function () {
      console.log('10');
    });
    new Promise(function (resolve) {
      console.log('11');
      resolve();
    }).then(function () {
      console.log('12');
    });
  }, 1000);
  // 1，7，6，8，2，4，3，5，9，11，10，12
  ```

  > 写的比较简略，详情请看参考:[掘金——这一次，彻底弄懂 JavaScript 执行机制](https://segmentfault.com/a/1190000018227028)

#### 浅拷贝 深拷贝

- 数据分为基本数据类型(String,Number,Boolean,Null,Undefined)和引用数据类型(Object,Array,Function)
- 对于基本数据类型，浅拷贝深拷贝都是拷贝值，但对于引用数据类型，他们有以下区别
  - 浅拷贝只复制某个对象的地址，新旧对象还是共享同一块内存
  - 深拷贝会开辟一块新的内存空间，修改新对象不会改到原对象
- 浅拷贝和赋值的区别

  | type   | 和原数据是否指向同一对象 | 第一层数据为基本数据类型 | 原数据中包含子对象       |
  | ------ | ------------------------ | ------------------------ | ------------------------ |
  | 赋值   | 是                       | 改变会使原数据一同改变   | 改变会使原数据一同改变   |
  | 浅拷贝 | 否                       | 改变不会使原数据一同改变 | 改变会使原数据一同改变   |
  | 深拷贝 | 否                       | 改变不会使原数据一同改变 | 改变不会使原数据一同改变 |

- 常见浅拷贝方法
  - 扩展运算符 ...
  - Array.prototype.concat()
  - Object.assign() `当Object只有一层的时候是深拷贝`
- 常见深拷贝方法
  - JSON.parse(JSON.stringify()) `但是JSON.stringify无法处理函数`
  - lodash 方法 `lodash.cloneDeep()`
  - 利用递归实现

```js
// 精简
const deepClone = (obj) => {
  let clone = Object.assign({}, obj);
  Object.keys(clone).forEach(
    (key) => (clone[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key])
  );
  return Array.isArray(obj) ? (clone.length = obj.length) && Array.from(clone) : clone;
};

// 易读
function deepClone(obj) {
  let clone = Object.assign({}, obj);
  Object.keys(clone).forEach((item) => {
    if (typeof clone[item] === 'object') {
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

> - 参考：[知乎——javascript 中的深拷贝和浅拷贝？](https://www.zhihu.com/question/23031215)
> - 参考：[掘金——浅拷贝与深拷贝](https://juejin.im/post/5b5dcf8351882519790c9a2e)
> - 参考：[掘金——深拷贝 vs 浅拷贝](https://juejin.im/post/59ac1c4ef265da248e75892b)

#### 防抖、节流

- 防抖和节流的作用都是防止函数多次调用
- 防抖：任务频繁触发的情况下，只有任务触发的间隔超过指定间隔的时候，任务才会执行。
  - 应用场景(执行最后一次)
    - input 在 onchange 时候的触发(用户输入验证/搜索等)
    - 按钮重复点击(也是一种场景，不过我习惯用 btnLoading)
  - 代码实现
    ```js
    // 简单实现
    const debounce = (fn, ms = 0) => {
      let timeoutId;
      return function (...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => fn.apply(this, args), ms);
      };
    };
    // 参考二有更好的方式
    ```
- 节流：指定时间间隔内只会执行一次任务；
  - 应用场景(有间隔地持续执行)
    - 鼠标滚动时会执行的函数
    - 窗口 resize
    - 拖拽(slider)
  - 代码实现
  ```js
  const throttle = (fn, ms) => {
    let canRun = true;
    let timer;
    let lastTime = Date.now();
    return function (...args) {
      if (canRun) {
        fn.apply(this, args);
        canRun = false;
      } else {
        clearTimeout(timer);
        timer = setTimeout(() => {
          if (Date.now() - lastTime >= ms) {
            fn.apply(this, args);
            lastTime = Date.now();
          }
        }, Math.max(ms - (Date.now() - lastTime), 0));
      }
    };
  };
  ```

> - 参考：[掘金——函数节流与函数防抖](https://juejin.im/entry/58c0379e44d9040068dc952f)
> - 参考：[InterviewMap——防抖](https://yuchengkai.cn/docs/frontend/#%E9%98%B2%E6%8A%96)
> - 参考：[github issues——节流和防抖的个人见解](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/5)

#### 柯里化

- 是高阶函数的一种特殊用法
- 定义：接收函数 A 作为参数，运行后能够返回一个新的函数，并且这个新的函数能够处理函数 A 的剩余参数。
- 实现
  - lodash.curry()
  - 简单实现，参数只能从右到左传递
    ```js
    function createCurry(func, args) {
      var arity = func.length;
      var args = args || [];
      return function () {
        var _args = [].slice.call(arguments);
        [].push.apply(_args, args);
        // 如果参数个数小于最初的func.length，则递归调用，继续收集参数
        if (_args.length < arity) {
          return createCurry.call(this, func, _args);
        }
        // 参数收集完毕，则执行func
        return func.apply(this, _args);
      };
    }
    ```
  - 30 seconds of code:
    ```js
    const curry = (fn, arity = fn.length, ...args) =>
      arity <= args.length ? fn(...args) : curry.bind(null, fn, arity, ...args);
    ```
- 例子
  ```js
  function add(a, b, c) {
    return a + b + c;
  }
  createCurry(add)(1, 2, 3); // 6
  createCurry(add)(1, 2)(3); // 6
  createCurry(add)(1)(2)(3); // 6
  curry(add)(1)(2)(3); // 6
  curry(add)(1)(2, 3); // 6
  ```
  > - 参考：[llh911001——JS 函数式编程指南](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html#%E4%B8%8D%E4%BB%85%E4%BB%85%E6%98%AF%E5%8F%8C%E5%85%B3%E8%AF%AD%E5%92%96%E5%96%B1)
  > - 参考：[简书——深入详解函数的柯里化](https://www.jianshu.com/p/5e1899fe7d6b)
  > - 参考：[segmentfault——简述几个非常有用的柯里化函数使用场景](https://segmentfault.com/a/1190000015281061)

#### 常见设计模式

- 单例模式
  - 保证一个类仅有一个实例，并提供一个访问它的全局访问点。实现的方法为先判断实例存在与否，如果存在则直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。
  - 适用场景：一个单一对象，比如：弹窗，无论点击多少次，弹窗只应该被创建一次
- 策略模式
  - 定义一系列的算法，把他们一个个封装起来，并且使他们可以相互替换。
  - 根据不同参数可以命中不同的策略
- 代理模式
  - 为一个对象提供一个代用品或占位符，以便控制对它的访问。
  - 代理对象和本体对象具有一致的接口
  - 适用场景：图片懒加载
- 中介者模式
  - 对象和对象之间借助第三方中介者进行通信
  - 例如飞机只需要和塔台通信就知道其他飞机的状态，不需要和所有飞机通信
  - 适用场景：商城购买，多种场景都会触发 onchange
- 装饰者模式
  - 在不改变对象自身的基础上，在程序运行期间给对象动态地添加方法。
  - 举例：redux connect 与 Hoc

> 参考：[JavaScript 中常见设计模式整理](https://juejin.im/post/6844903607452581896)
> 参考：[JavaScript 设计模式](https://juejin.im/post/6844903503266054157)

## React

React 是一个声明式，用于构建用户界面的 JS 库。使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。

- 声明式设计
- 使用 VDOM，减少 DOM 的交互
- JSX 语法
- 组件化 代码复用

#### 生命周期

- 创建阶段
  - constructor
  - static getDerivedStateFromProps
  - render
  - componentDidMount
- 更新阶段
  - static getDerivedStateFromProps
  - shouldComponentUpdate
  - render
  - getSnapshotBeforeUpdate
  - componentDidUpdate
- 卸载阶段 - componentWillUnmount
  > - componentWillMount、componentWillUpdate、componentWillReceiveProps 是遗留的方法，在 v17.\*\*大版本中就不能用了
  > - getDerivedStateFromProps，用的时候要比较注意。[官网——You Probably Don't Need Derived State](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)
  > - 参考：[官网——React.Component](https://reactjs.org/docs/react-component.html)
  > - 参考：[官网——生命周期图](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

#### 组件之间怎么通信

- 父级传给子级：this.props
- 子级传给父级：ref
- 跨组件传递：context，redux

#### react 中 key 的作用是什么

- 为了在 diff 算法执行时更快的找到对应的节点，提高 diff 速度
- key 变化的时候，节点会重新渲染
- map 的时候尽量不要用 index 做 key 值
  > 举例： 如果希望可以在 B 和 C 之间加一个 F
  > Diff 算法默认执行起来是这样的
  > ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/89713889899F496AB5A92E2A52B8A163/5375)
  > 即把 C 更新成 F，D 更新成 C，E 更新成 D，最后再插入 E，是不是很没有效率？<br />
  > 所以可是使用 key 来给每个节点做一个唯一标识，Diff 算法就可以正确的识别此节点，找到正确的位置区插入新的节点。<br /> > ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/8E6617DF1C4E47D480CF5B377EF2A863/5403)

#### diff 算法

- render 执行的结果得到的不是真正的 DOM 节点，仅仅是轻量级的 JavaScript 对象, 我们称之为 virtual DOM.

react 16.0 之前和之后的 Diff 算法有很大不同，[老的 Diff 算法](https://zhuanlan.zhihu.com/p/20346379)比较复杂，16.0 版本发布了`React Fiber`。Fiber 是对 React 核心算法的一次重新实现，将原本的同步更新过程碎片化，避免主线程的长时间阻塞，使应用的渲染更加流畅

在 React16 之前，更新组件时会调用各个组件的生命周期函数，计算和比对 Virtual DOM，更新 DOM 树等，这整个过程是同步进行的，中途无法中断。当组件比较庞大，更新操作耗时较长时，就会导致浏览器唯一的主线程都是执行组件更新操作，而无法响应用户的输入或动画的渲染，很影响用户体验。

Fiber 利用分片的思想，把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，在每个小片执行完之后，就把控制权交还给 React 负责任务协调的模块，如果有紧急任务就去优先处理，如果没有就继续更新，这样就给其他任务一个执行的机会，唯一的线程就不会一直被独占。

在 React Fiber 中，一次更新过程会分成多个分片完成，所以完全有可能一个更新任务还没有完成，就被另一个更高优先级的更新过程打断，这时候，优先级高的更新任务会优先处理完，而低优先级更新任务所做的工作则会完全作废，然后等待机会重头再来。

因为一个更新过程可能被打断，所以 React Fiber 一个更新过程被分为两个阶段(Phase)：第一个阶段 Reconciliation Phase 和第二阶段 Commit Phase。

Fiber： 工作单元、维护每一个分片的数据结构、一种解决可中断的调用任务的一种解决方案

React16 的 diff 策略采用从链表头部开始比较的算法，是层次遍历，算法是建立在一个节点的插入、删除、移动等操作都是在节点树的同一层级中进行的。
对于 Diff， 新老节点的对比，我们以新节点为标准，然后来构建整个 currentInWorkProgress，对于新的 children 会有四种情况。

- TextNode(包含字符串和数字)
- 单个 React Element(通过该节点是否有 ?typeof 区分)
- 数组
- 可迭代的 children，跟数组的处理方式差不多

> - 参考：[Deep In React 之浅谈 React Fiber 架构(一)](https://mp.weixin.qq.com/s/dONYc-Y96baiXBXpwh1w3A)
> - 参考：[Deep In React 之详谈 React 16 Diff 策略(二)](https://segmentfault.com/a/1190000019918133)
> - 参考：[老的 react diff：React 源码剖析系列 － 不可思议的 react diff](https://zhuanlan.zhihu.com/p/20346379)
> - 参考：[segmentfault——React 的 diff 算法](https://segmentfault.com/a/1190000000606216)
> - 参考：[React Fiber 是什么](https://zhuanlan.zhihu.com/p/26027085)

#### function Component / class Component todo

#### context

- React.createContext
- `<MyContext.Provider value={/* some value */}>`
- `MyClass.contextType = MyContext;` 可以写一个 HOC，这个写在 HOC 中，就不需要每个用到的都写一遍
- MyContext.Provider
- MyContext.Consumer

#### hooks

- useState
- useEffect
- useContext
- useReducer
- useMemo
- useRef
- 自定义 hook 与普通函数一样，只是:
  - 名称以 “use” 开头
  - 函数内部可以调用其他的 Hook

#### react hooks todo

#### redux

- store: 将整个应用的 state 储存在唯一的 store 中。
- state 只能通过触发 action 来修改，其中 action 就是一个描述性的普通对象。
- reducer: 接收 action 和当前 state 作为参数，返回一个新的 State

##### 使用 redux 时，常用到的 api

- redux.createStore: `createStore(reducer, middleware)`
- redux.combineReducers
- store.getState
- store.dispatch
- [react-redux].connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])

> 参考：[Redux 入门教程，应用的状态管理器](https://www.jianshu.com/p/d296a8c34936)

##### redux 数据流的走向

![](https://raw.githubusercontent.com/dream-approaching/pictureMaps/master/img/20200915175323.png)

- 用户发出 Action: store.dispatch(action);
- Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action
- Reducer 会返回新的 State
- State 一旦有变化，Store 就会调用监听函数
- listener 可以通过 store.getState()得到当前状态，触发重新渲染 View

##### redux 和 mobx 的区别

- redux
  - 单一 store
  - 状态对象不可变
  - 优点：后期易维护
  - 缺点：繁琐、识点较多，上手相对较难
- mobx
  - 多个 store
  - 可直接修改对象
  - 优点：流程相对简单
  - 缺点：过于自由，后期不易维护

##### redux-saga 和 redux-thunk 对比

- redux-saga
  - dispatch 一个简单对象，利用 generator 的思想处理异步
  - 优点: 异步控制清晰，并发处理方便
  - 缺点: 代码书写较多，上手较慢
- redux-thunk
  - 在 UI 组件中触发任务，dispatch 一个 action 生成函数，在这个函数中处理异步
  - 优点: 简单
  - 缺点: action 变得复杂，后期可维护性降低, 协调并发任务比较困难

##### redux-saga 常用的 api

- createSagaMiddleware: 创建一个 Redux middleware，并将 Sagas 连接到 Redux Store。
- takeEvery: 在发起（dispatch）到 Store 并且匹配 pattern 的每一个 action 上派生一个 saga。
- take: 创建一个 Effect 描述信息，用来命令 middleware 在 Store 上等待指定的 action
- put: 用于触发 action，功能上类似于 dispatch。
- call: 用于调用异步逻辑，支持 promise 。
- select: 可以取到 state 数据

##### umi

定位是开发框架，包含工具 + 路由

- 优点
  - 开箱即用: webpack + react router
  - 做了很多优化: Tree Shaking、公共文件的智能提取、按需加载

> **FAQ: umi 有啥特别的，工具用 webpack + webpack-dev-server + babel + postcss + ... ，路由用 react-router 不就完了吗?**  
> 这是上一代的使用方式，工具是工具，库是库，泾渭分明。而近来，我们发现工具和库其实可以糅合在一起，工具也是框架的一部分。 通过约定、自动生成和解析代码等方式来辅助开发，减少开发者要写的代码量。next.js 如此，umi 也如此

> 参考: [Hello！umi](https://zhuanlan.zhihu.com/p/33455048)

#### 组件的复用

- High order component
- render props
- custom hooks

#### react 和 vue 的区别

- 监听数据变化的实现原理不同
  - Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
  - React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的 VDOM 的重新渲染
- 数据流的不同
  - Vue 可以实现双向绑定
  - React 一直提倡的是单向数据流
- HoC 和 mixins
  - 在 Vue 中我们组合不同功能的方式是通过 mixin
  - 而在 React 中我们通过 HoC (高阶组件）
- 组件通信的区别
  - 相邻的组件通信是相似的
  - 跨组件的传递中 vue 使用 provide/inject，react 使用 context
- 模板渲染方式的不同
  - vue 通过一种拓展的 HTML 语法进行渲染，也可以用 jsx
  - 通过 JSX 渲染

## RN todo

## 浏览器

#### 浏览器缓存

缓存是性能优化中非常重要的一环

浏览器中的缓存作用分为两种情况，一种是需要发送 HTTP 请求，一种是不需要发送。

- 强缓存。首先是检查强缓存，这个阶段不需要发送 HTTP 请求。
  - `HTTP/1.0`使用`Expires`检查强缓存，即过期时间，存在于服务端返回的响应头中，但服务器的时间和浏览器的时间可能并不一致。因此这种方式很快在后来的`HTTP1.1`版本中被抛弃了。
  - `HTTP1.1`使用`Cache-Control`检查强缓存，采用过期时长来控制缓存，对应的字段是 max-age。如`Cache-Control:max-age=3600`，代表这个响应返回后在 3600 秒，也就是一个小时之内可以直接使用缓存。
- 协商缓存。强缓存失效之后，浏览器在请求头中携带相应的缓存 tag 来向服务器发请求，由服务器根据这个 tag，来决定是否使用缓存，这就是协商缓存。缓存 tag 分为两种: `Last-Modified` 和 `ETag`。
  - Last-Modified
    - 即最后修改时间。在浏览器第一次给服务器发送请求后，服务器会在响应头中加上这个字段。
    - 浏览器再次请求，会在请求头中携带 If-Modified-Since 字段
    - 服务器拿到 If-Modified-Since，跟服务器中时间做对比判断是否要更新
  - ETag
    - 是服务器根据当前文件的内容，给文件生成的唯一标识，只要里面的内容有改动，这个值就会变。服务器通过响应头把这个值给浏览器。
    - 浏览器接收到 ETag 的值，会在下次请求时，将这个值作为 If-None-Match 这个字段的内容，并放到请求头中，然后发给服务器。
    - 服务器接收到 If-None-Match 后，会跟服务器上该资源的 ETag 进行比对，判断是否要更新
  - 对比
    - 在精准度上，ETag 优于 Last-Modified
    - 在性能上，Last-Modified 优于 ETag
    - 如果两种方式都支持的话，服务器会优先考虑 ETag
- 缓存位置  
  当强缓存命中或者协商缓存中服务器返回 304 的时候，我们直接从缓存中获取资源。缓存位置一共有四种，按优先级从高到低排列分别是
  - Service Worker：离线缓存就是 Service Worker Cache
  - Memory Cache：是内存缓存，从效率上讲它是最快的。但是从存活时间来讲又是最短的，当渲染进程结束后，内存缓存也就不存在了。
  - Disk Cache：是存储在磁盘中的缓存，从存取效率上讲是比内存缓存慢的，但是他的优势在于存储容量和存储时长。
  - Push Cache：即推送缓存，这是浏览器缓存的最后一道防线。它是 HTTP/2 中的内容

> 总结：首先通过 Cache-Control 验证强缓存是否可用，如果强缓存可用，直接使用，否则进入协商缓存，即发送 HTTP 请求，服务器通过请求头中的 If-Modified-Since 或者 If-None-Match 字段检查资源是否更新，若资源更新，返回资源和 200 状态码，否则，返回 304，告诉浏览器直接从缓存获取资源。

> 参考：[第 1 篇: 能不能说一说浏览器缓存?](https://juejin.im/post/6844904021308735502#heading-0)

#### 浏览器的本地存储 session、cookie、localstorage、sessionStorage、IndexDB

- cookie：保存在浏览器端， 大小 4K，可以设置过期时间
- session：保存在服务器端，无大小限制
- localstorage：除非手动清除，否则永久保存，大小 5M
- sessionStorage：与 localStorage 类似，不过浏览器关闭时会自动清空
  ![](https://user-gold-cdn.xitu.io/2018/12/14/167ac245e669b3b3?imageslim)
- IndexDB: 支持的浏览器比较广泛，大小限制通常约为 50MB。

#### 输入一个网址到页面展示，发生了什么事情

- DNS 解析:将域名解析成 IP 地址
- TCP 连接：TCP 三次握手
- 发送 HTTP 请求
- 服务器处理请求(期间可能会读取服务器缓存或查询数据库)，并返回响应报文
- 浏览器根据返回的状态码判断是下载还是从缓存读取文件内容
- 浏览器解析渲染页面
- 断开连接：TCP 四次挥手
  > - 参考：[掘金——浏览器基础](https://juejin.im/post/5c137e7c6fb9a049f7461639#heading-2)
  > - 参考：[掘金——从 URL 输入到页面展现到底发生什么？](https://juejin.im/post/5bf3ad55f265da61682afc9b)

#### TCP 三次握手 四次挥手

- 三次握手
  - 客户端发送 SYN 报文
  - 服务端发送 SYN+ACK 报文
  - 客户端再发送 ACK 确认包
- 四次挥手
  - 主动方发送 FIN 报文
  - 接收方收到 FIN，发回一个 ACK 确认
  - 接收方也发送一个 FIN
  - 主动方收到后发回 ACK 确认

> - 参考：动画讲解：[掘金——三次握手 四次挥手](https://juejin.im/post/5b29d2c4e51d4558b80b1d8c)
> - 参考：[博客园——为什么不是 2 次握手或者其他次](https://www.cnblogs.com/zhuzhenwei918/p/7465467.html)

#### 常见状态码

- 1xx 信息相关
- 2XX 成功
  - 200 OK，成功
  - 204 No content，表示请求成功，但没有返回任何内容
- 3XX 重定向
  - 301 moved permanently，永久性重定向，表示资源已被分配了新的 URL
  - 302 found，临时性重定向，表示资源临时被分配了新的 URL
  - 304 not modified，表示未修改，读取缓存
- 4XX 客户端错误
  - 400 bad request，请求报文存在语法错误
  - 401 unauthorized，需要身份验证
  - 403 forbidden，表示对请求资源的访问被服务器拒绝
  - 404 not found，表示在服务器上没有找到请求的资源
- 5XX 服务器错误
  - 500 internal sever error，表示服务器端在执行请求时发生了错误
  - 503 service unavailable，服务不可用

> - 记住上面这些面试应该够了，工作需要其他的直接去 google 吧~

#### http 请求头里都有什么内容

![](https://raw.githubusercontent.com/dream-approaching/pictureMaps/master/img/20200912165539.png)

- `Accept` 可接受的响应内容类型（Content-Types）。
- `Accept-Encoding` 可接受的响应内容的编码方式。
- `Accept-Language` 可接受的响应内容语言列表。
- `Connection` 客户端（浏览器）想要优先使用的连接类型
- `Cookie` Cookie
- `Host` 主机
- `Origin` 表示访问控制所允许的来源
- `Referer` 表示浏览器所访问的前一个页面，
- `User-Agent` 浏览器的身份标识字符串

#### http 和 https 的区别

- HTTPS 协议需要到 CA 申请证书，一般免费证书很少，需要交费
- HTTP 协议运行在 TCP 之上，所有传输的内容都是明文，HTTPS 运行在 SSL/TLS 之上，SSL/TLS 运行在 TCP 之上，所有传输的内容都经过加密的。
- HTTP 和 HTTPS 端口也不一样，前者是 80，后者是 443。
- HTTPS 可以有效的防止运营商劫持
- HTTPS 比 HTTP 更加安全，对搜索引擎更友好，利于 SEO

#### http 1.0、http 1.1 和 http 2.0 的区别

- http1.0
  - 缺陷：浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个 TCP 连接（TCP 连接的新建成本很高，因为需要客户端和服务器三次握手），服务器完成请求处理后立即断开 TCP 连接，服务器不跟踪每个客户也不记录过去的请求；
  - 解决方案：添加头信息——非标准的 Connection 字段 Connection: keep-alive
- http1.1
  - 改进点
    - 持久连接：引入了持久连接，即 TCP 连接默认不关闭，可以被多个请求复用，不用声明 Connection: keep-alive(对于同一个域名，大多数浏览器允许同时建立 6 个持久连接)
    - 管道机制：在同一个 TCP 连接里面，客户端可以同时发送多个请求
    - 分块传输编码：服务端没产生一块数据，就发送一块，采用”流模式”而取代”缓存模式”。
    - 新增请求方式：put,delete,options,trace,connect
  - 缺点
    - 虽然允许复用 TCP 连接，但是同一个 TCP 连接里面，所有的数据通信是按次序进行的。服务器只有处理完一个请求，才会接着处理下一个请求。如果前面的处理特别慢，后面就会有许多请求排队等着。这将导致“队头堵塞”
    - 避免方式：一是减少请求数，二是同时多开持久连接
- http 2.0
  - 二进制协议：HTTP/2 则是一个彻底的二进制协议，头信息和数据体都是二进制，并且统称为”帧”：头信息帧和数据帧。
  - 完全多路复用：HTTP/2 复用 TCP 连接，在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，而且不用按照顺序一一对应，这样就避免了”队头堵塞”。
  - 报头压缩
  - 服务器推送：允许服务器未经请求，主动向客户端发送资源
- 总结
  | http1.0 | http1.1 | http2.0 |
  | :------------: | :--------------------------------------------------------------------------: | :------------------------------------------------------------------: |
  | 无状态、无连接 | 持久连接<br/>请求管道化<br/>加缓存处理<br/>增加 Host 字段<br/>支持断点传输等 | 二进制分帧 <br/> 多路复用（或连接共享）<br/> 头部压缩<br/>服务器推送 |
  > - 参考：[HTTP1.0 HTTP1.1 HTTP2.0 主要特性对比/2](https://segmentfault.com/a/1190000013028798)
  > - 参考：[HTTP1.0、HTTP1.1 和 HTTP2.0 的区别](https://juejin.im/entry/6844903489596833800)
  > - 参考：[HTTP/2 服务器推送（Server Push）教程](http://www.ruanyifeng.com/blog/2018/03/http2_server_push.html)
  > - 参考：[深入理解 http1.x、http 2 和 https](https://segmentfault.com/a/1190000015316332)
  > - 参考：[怎样把网站升级到 http/2](https://zhuanlan.zhihu.com/p/29609078)

#### http 3.0

- 2018 年，QUIC 演变成为 HTTP3。
- 占比约 5%
- HTTP3 的主要改进在传输层上。不走 TCP 连接了，走 UDP。

> - 参考：[秒懂科普！HTTP 3 如此简单](https://segmentfault.com/a/1190000023929858)

#### 跨域以及常见解决办法

- 浏览器有一个安全策略叫同源策略，同源就是要求, 域名, 协议, 端口相同，任意一个不同则叫不同域
- 同源策略还会引起 Cookie、LocalStorage 和 IndexDB 无法读取
- 不同域之间 iframe, ajax 均受其限制, script 标签不受此限制.
- 解决跨域的方式

  - 使用代理
    - 请求同域下的 web 服务器;
    - web 服务器像代理一样去请求真正的第三方服务器;
    - 代理拿到数据过后, 直接返回给客户端 ajax.
  - jsonp: 即 JSON with padding。因为 script 标签是不会跨域的，利用这个特性，可以解决跨域，兼容性很好，但是只支持 GET 方式
    ```js
    //需要跨域的时候可以创建一个script标签
    var jsonp = document.createElement('script');
    //指定类型
    jsonp.type = 'text/javascript';
    //添加链接地址
    jsonp.src = 'http://10.0.154.249/text.js?callback=jsonpCallback';
    //将这个拼接 添加到head标签中。
    document.getElementsByTagName('head')[0].appendChild(jsonp);
    //回调函数
    function jsonpCallback(ret) {
      alert(ret);
    }
    ```
  - CORS：原理是使用自定义的 HTTP 头部，让服务器与浏览器进行沟通，主要是通过设置响应头的 Access-Control-Allow-Origin: \* 来达到目的
  - postMessage：postMessage(data,origin)
    ```js
    // 父页面发送消息
    window.frames[0].postMessage('message', origin);
    // 子页面监听自身的message事件来获取传过来的消息
    window.addEventListener('message', function (e) {
      if (e.source != window.parent) return; //若消息源不是父页面则退出
      //TODO ...
    });
    ```
  - window.name：在一个 window 的生命周期内,窗口载入的所有的页面都是共享一个 window.name 的，即当 window 的 location 变化并且重新加载,它的 name 属性可以依然保持不变.(还是要借助 iframe)
  - document.domain：不同的子域 + iframe 交互的时候，可以借助 document.domain 来解决。不过 document.domain 只能设置为页面本身或者更高一级的域名。 - location.hash：利用 url 的 hash，因为服务端不关心这部分。
    > - 有些不常用的，看不太懂，硬背下来吧
    > - 参考：[掘金——由同源策略到前端跨域](https://juejin.im/post/58f816198d6d81005874fd97)
    > - 参考：[segmentfault——浅谈浏览器端 JavaScript 跨域解决方法](https://segmentfault.com/a/1190000004518374)
    > - 参考：[简书——浏览器中使用 js 跨域获取数据的几种方式](https://www.jianshu.com/p/c71c20e98f94)

#### XSS 攻击

- 全称是 Cross Site Scripting(即跨站脚本) 为了和 CSS 区分，故叫它 XSS
- 攻击的实现有三种方式
  - 存储型：常见的场景是留言评论区提交一段脚本代码，如果前后端没有做好转义的工作，那评论内容存到了数据库，在页面渲染过程中直接执行, 相当于执行一段未知逻辑的 JS 代码
  - 反射型：如`http://sanyuan.com?q=<script>alert("你完蛋了")</script>`
  - 文档型：WIFI 路由器劫持或者本地恶意软件
- 防范措施：千万不要相信任何用户的输入！
  - 无论是在前端和服务端，都要对用户的输入进行转码或者过滤
  - 利用 CSP：即浏览器中的内容安全策略，它的核心思想就是服务器决定浏览器加载哪些资源
  - 利用 HttpOnly：很多 XSS 攻击脚本都是用来窃取 Cookie, 而设置 Cookie 的 HttpOnly 属性后，JavaScript 便无法读取 Cookie 的值。这样也能很好的防范 XSS 攻击。

#### CSRF 攻击

- CSRF(Cross-site request forgery), 即跨站请求伪造，指的是黑客诱导用户点击链接，打开黑客的网站，然后黑客利用用户目前的登录状态发起跨站请求。
- 防范措施

  - 利用 Cookie 的 SameSite 属性：SameSite 可以设置为三个值，Strict、Lax 和 None
  - 验证来源站点：需要要用到请求头中的两个字段: Origin 和 Referer。
  - 校验 token

## 其他

#### 前端模块化

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

##### ES6 模块与 CommonJS 模块的差异

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

#### 前端工程化

个人理解: 软件是从无到有的，前端工程化是指在开发过程中，提高开发效率，减少后期维护成本的一个方案。它应该考虑以下几个因素：

- 模块化
- 组件化
- 规范化(目录结构, 统一的编码规范, 文档规范, git 分支管理, mock 数据, 接口返回格式)
- 自动化(自动化测试, 自动化部署)

#### 重绘和回流（重绘和重排）

- 回流也叫重排
- 当对 DOM 结构的修改引发 DOM 几何尺寸变化的时候，会发生回流的过程
  - 一个 DOM 元素的几何属性变化，常见的几何属性有 width、height、padding、margin、left、top、border 等等
  - 使 DOM 节点发生增减或者移动
  - 读写 offset 族、scroll 族和 client 族属性的时候，浏览器为了获取这些值，需要进行回流操作
  - 调用 window.getComputedStyle 方法
- 当 DOM 的修改导致了样式的变化，并且没有影响几何属性的时候，会导致重绘(repaint)
  - 例如改变元素背景色时
  - visibility: hidden 隐藏一个 DOM 节点-只触发重绘
- 重绘不一定导致回流，但回流一定发生了重绘。
- 如何减少重绘和回流

  - 不要逐个变样式

    ```js
    // bad
    var left = 10,
      top = 10;
    el.style.left = left + 'px';
    el.style.top = top + 'px';
    // better
    el.className += ' theclassname';
    // 当top和left的值是动态计算而成时...
    // better
    el.style.cssText += '; left: ' + left + 'px; top: ' + top + 'px;';
    ```

  - 不要频繁计算样式
    ```js
    // no-no!
    for (big; loop; here) {
      el.style.left = el.offsetLeft + 10 + 'px';
      el.style.top = el.offsetTop + 10 + 'px';
    }
    // better
    var left = el.offsetLeft,
      top = el.offsetTop;
    esty = el.style;
    for (big; loop; here) {
      left += 10;
      top += 10;
      esty.left = left + 'px';
      esty.top = top + 'px';
    }
    ```
  - 对于 resize、scroll 等进行防抖/节流处理
  - 使用[createDocumentFragment](https://www.jianshu.com/p/8ae83364c09c)
    > - 参考：[翻译计划-重绘重排重渲染](https://xdlrt.github.io/2016/11/05/2016-11-05/)
    > - 参考：[谈谈你对重绘和回流的理解](https://juejin.im/post/6844904021308735502#heading-54)
