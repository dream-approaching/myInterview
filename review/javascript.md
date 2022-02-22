<!-- TOC -->

- [1. javascript](#1-javascript)
    - [1.1. 冒泡/捕获](#11-冒泡捕获)
    - [1.2. 跨浏览器的事件处理程序](#12-跨浏览器的事件处理程序)
    - [1.3. 事件委托](#13-事件委托)
    - [1.4. 原型链](#14-原型链)
    - [1.5. 实现继承的几种方式](#15-实现继承的几种方式)
        - [1.5.1. 优缺点对比：](#151-优缺点对比)
    - [1.6. new 内部的原理](#16-new-内部的原理)
    - [1.7. instanceof / typeof](#17-instanceof--typeof)
    - [1.8. this、call、apply、bind](#18-thiscallapplybind)
    - [1.9. 作用域](#19-作用域)
    - [1.10. 闭包](#110-闭包)
    - [1.11. 造成内存泄漏的常见情况](#111-造成内存泄漏的常见情况)
    - [1.12. 堆栈的区别](#112-堆栈的区别)
    - [1.13. ES6](#113-es6)
        - [1.13.1. var 和 const let 区别](#1131-var-和-const-let-区别)
        - [1.13.2. Promise](#1132-promise)
        - [1.13.3. async await](#1133-async-await)
        - [1.13.4. generator](#1134-generator)
        - [1.13.5. Set / Map](#1135-set--map)
        - [1.13.6. Proxy](#1136-proxy)
        - [1.13.7. Symbol](#1137-symbol)
        - [1.13.8. Interator / for of](#1138-interator--for-of)
        - [1.13.9. Object 扩展](#1139-object-扩展)
        - [1.13.10. Array 扩展](#11310-array-扩展)
    - [1.14. Array](#114-array)
    - [1.15. for in / for of](#115-for-in--for-of)
    - [1.16. 事件循环(EventLoop)](#116-事件循环eventloop)
        - [1.16.1. 宏任务，微任务](#1161-宏任务微任务)
    - [1.17. 浅拷贝 深拷贝](#117-浅拷贝-深拷贝)
    - [1.18. 防抖、节流](#118-防抖节流)
    - [1.19. 重绘和回流（重绘和重排）](#119-重绘和回流重绘和重排)
    - [1.20. 柯里化](#120-柯里化)
    - [1.21. 常见设计模式](#121-常见设计模式)

<!-- /TOC -->
# 1. javascript

## 1.1. 冒泡/捕获

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

## 1.2. 跨浏览器的事件处理程序

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

## 1.3. 事件委托

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

## 1.4. 原型链

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

## 1.5. 实现继承的几种方式

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

### 1.5.1. 优缺点对比：

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

## 1.6. new 内部的原理

```js
var a = new myFunction("Li","Cherry");
new myFunction{
    var obj = {}; // 创建一个空对象 obj
    obj.__proto__ = myFunction.prototype; // 将新创建的空对象的隐式原型指向其构造函数的显示原型
    var result = myFunction.call(obj,"Li","Cherry"); // 使用 call 改变 this 的指向
    return typeof result === 'object'? result : obj; // 如果无返回值或者返回一个非对象值，则将 obj 返回作为新对象；如果返回值是一个新对象的话那么直接直接返回该对象
}
```

## 1.7. instanceof / typeof

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

## 1.8. this、call、apply、bind

- 下面的文章写得好，多看几遍，懒得总结了
- 参考：[掘金——this、apply、call、bind](https://juejin.im/post/59bfe84351882531b730bac2)
- 参考：[掘金——this（他喵的）到底是什么 — 理解 JavaScript 中的 this、call、apply 和 bind](https://juejin.im/post/5b9f176b6fb9a05d3827d03f)

## 1.9. 作用域

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

## 1.10. 闭包

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

## 1.11. 造成内存泄漏的常见情况

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

## 1.12. 堆栈的区别

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

## 1.13. ES6

### 1.13.1. var 和 const let 区别

- 块级作用域
- 不存在变量提升
- 暂时性死区
- 不可重复声明
- let、const 声明的  全局变量不会挂在顶层对象下面
- const 声明之后必须马上赋值，否则会报错
- const 简单类型一旦声明就不能再更改， 复杂类型(数组、对象等)指针指向的地址不能更改，内部数据可以更改

### 1.13.2. Promise

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

### 1.13.3. async await

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

### 1.13.4. generator

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

### 1.13.5. Set / Map

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

### 1.13.6. Proxy

- 在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截
- ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例
  `var proxy = new Proxy(target, handler);`
  > 能记这些就差不多了，再问直接放弃

### 1.13.7. Symbol

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

### 1.13.8. Interator / for of

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

### 1.13.9. Object 扩展

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

### 1.13.10. Array 扩展

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

## 1.14. Array

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

## 1.15. for in / for of

- 推荐在循环对象属性的时候，使用 for...in,在遍历数组的时候的时候使用 for...of。
- for...in 循环出的是 key，for...of 循环出的是 value
- 注意，for...of 是 ES6 新引入的特性。修复了 ES5 引入的 for...in 的不足
- for...of 不能循环普通的对象，需要通过和 Object.keys()搭配使用

> 参考 [javascript 中 for of 和 for in 的区别？](https://segmentfault.com/q/1010000006658882)

## 1.16. 事件循环(EventLoop)

![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/CDE1E05AA6924122A3BC9793CF2C6D0E/5042)

- 同步和异步任务分别进入不同的执行"场所"
- 同步的进入主线程，异步的进入 Event Table 并注册函数。
- 当指定的事情完成时，Event Table 会将这个函数移入 Event Queue。
- 主线程内的任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的 Event Loop(事件循环)。

### 1.16.1. 宏任务，微任务

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

## 1.17. 浅拷贝 深拷贝

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

## 1.18. 防抖、节流

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


## 1.19. 重绘和回流（重绘和重排）

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


## 1.20. 柯里化

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

## 1.21. 常见设计模式

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
