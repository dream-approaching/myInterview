

<!-- TOC -->

- [html + css](#html--css)
    - [css 盒模型 todo](#css-盒模型-todo)
    - [用 border 绘制三角形](#用-border-绘制三角形)
    - [BFC todo](#bfc-todo)
    - [垂直居中 todo](#垂直居中-todo)
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
    - [ES6](#es6)
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
    - [for in / for of todo](#for-in--for-of-todo)
    - [事件循环(EventLoop)](#事件循环eventloop)
        - [宏任务，微任务](#宏任务微任务)
    - [递归 todo](#递归-todo)
    - [浅拷贝 深拷贝](#浅拷贝-深拷贝)
    - [防抖、节流](#防抖节流)
    - [柯里化](#柯里化)
- [React](#react)
    - [生命周期](#生命周期)
    - [组件之间怎么通信](#组件之间怎么通信)
    - [react 中 key 的作用是什么](#react-中-key-的作用是什么)
    - [diff 算法](#diff-算法)
    - [function Component / class Component todo](#function-component--class-component-todo)
    - [Controlled Component / Uncontrolled Component todo](#controlled-component--uncontrolled-component-todo)
    - [ref todo](#ref-todo)
    - [react hooks todo](#react-hooks-todo)
    - [react-redux todo](#react-redux-todo)
    - [组件的复用](#组件的复用)
- [RN todo](#rn-todo)
- [浏览器](#浏览器)
    - [session、cookie、localstorage、sessionStorage、IndexDB](#sessioncookielocalstoragesessionstorageindexdb)
    - [输入一个网址到页面展示，发生了什么事情](#输入一个网址到页面展示发生了什么事情)
    - [TCP 三次握手 四次挥手](#tcp-三次握手-四次挥手)
    - [常见状态码](#常见状态码)
    - [http 请求头里都有什么内容 todo](#http-请求头里都有什么内容-todo)
    - [http 和 https 的区别](#http-和-https-的区别)
    - [http 1.0、http 1.1 和 http 2.0 的区别](#http-10http-11-和-http-20-的区别)
    - [http 3.0](#http-30)
    - [跨域以及常见解决办法](#跨域以及常见解决办法)
- [其他](#其他)
    - [对前端模块化的理解 todo](#对前端模块化的理解-todo)
    - [CI/CD todo](#cicd-todo)

<!-- /TOC -->

## html + css

#### css 盒模型 todo

#### 用 border 绘制三角形

```
width: 0;
height: 0;
border: 30px solid;
border-color: transparent transparent lightblue;
```

> 参考： [简书——CSS 绘制三角形](https://www.jianshu.com/p/9a463d50e441)

#### BFC todo

#### 垂直居中 todo

## javascript

#### 冒泡/捕获

* DOM2 级事件规定的事件流包括三个阶段:
  * 事件捕获阶段
  * 处于目标阶段
  * 事件冒泡阶段</br>
    ![](https://user-images.githubusercontent.com/25027560/38007715-4cc457d0-327d-11e8-9fb3-667fa75fc38c.png)
* 阻止冒泡
  ```
  function stopPropagation(event){
    if(event.stopPropagation){
       event.stopPropagation();
    }else{
       event.cancelBubble=true;
    }
  },
  ```

#### 跨浏览器的事件处理程序

* 实现

  ```
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

> * 参考：[博客园——跨浏览器的事件对象](https://www.cnblogs.com/hykun/p/EventUtil.html)

#### 事件委托

* 用来解决事件处理程序过多的问题
* 使用场景
  ```
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

1.  实例的**proto** === 其构造函数的 prototype
    ![](https://note.youdao.com/yws/public/resource/5decefaed3b17cd4bab92965ace4d207/xmlnote/36A4E7356D3A42A6A8655EEEF65CEEB2/4117)

    ```
    function Person(name) {
        this.name = name
    }
    let p = new Person('longzi');
    console.log(p.__proto__) // Person.prototype
    console.log(Person.__proto__) // Function.prototype
    ```

2.  `Function.__proto__ === Function.prototype`</br>
    ![](https://note.youdao.com/yws/public/resource/5decefaed3b17cd4bab92965ace4d207/xmlnote/E02A3DBFB32641C7A10EBC8982CCB875/4114)
3.  原型链的最顶层都是 null

> * 参考：这篇写的很好 多看几遍：[知乎——JavaScript 世界万物诞生记](https://zhuanlan.zhihu.com/p/22989691)
> * 参考：这篇也可以 作为上面的补充：[CSDN——prototype、**proto**与 constructor](https://blog.csdn.net/cc18868876837/article/details/81211729)

#### 实现继承的几种方式

* 假设有父类 Parent

  ```
  function Parent(name, age) {
    this.name = name;
    this.age = age;
    this.colors = ["red","blue","yellow"];
  }
  Parent.prototype.showName = function() {
    console.log(this.name);
  }
  ```

* 原型链继承
  ```
  function Child() {
    this.type = 'child';
  }
  Child.prototype = new Parent();
  var a = new Child()
  var b = new Child()
  console.log(a.showName()) // undefined
  console.log(a.colors) // ["red","blue","yellow"]
  console.log(b.colors) // ["red","blue","yellow"]
  a.colors.push('black')
  console.log(a.colors) // ["red","blue","yellow","black"]
  console.log(b.colors) // ["red","blue","yellow","black"]
  ```
* 构造函数继承
  ```
  function Child(name, age){
      Parent.call(this, name, age); // 或apply
  }
  var child1 = new Child('longzi', 23);
  var child2 = new Child('xiaofeng', 18);
  child1.colors.push('black');
  console.log(child1.colors) // ["red", "blue", "yellow", "black"]
  console.log(child2.colors) //["red", "blue", "yellow"]
  console.log(child1.name, child1.age)  // longzi, 23
  console.log(child2.name, child2.age)  // xiaofeng, 18
  console.log(child1.showName()) // Uncaught TypeError: child1.showName is not a function
  ```
* 组合继承(原型链+构造函数)
  ```
  function Child(name, age){
      Parent.call(this, name, age); // 构造函数继承
  }
  Child.prototype = new Parent();  
  var child1 = new Child('longzi', 23);
  var child2 = new Child('xiaofeng', 18);
  child1.colors.push('black');
  console.log(child1.colors) // ["red", "blue", "yellow", "black"]
  console.log(child2.colors) //["red", "blue", "yellow"]
  console.log(child1.showName()) // longzi
  ```
* Object.create() (网上所谓的寄生组合式继承，太 tm 抽象了，听了名字就害怕)
  ```
  function Child(name, age){
      Parent.call(this, name, age); // 构造函数继承
  }
  Child.prototype = Object.create(Parent.prototype);  
  Child.prototype.constructor = Child; // 这步不要忘了
  var child1 = new Child('longzi', 23);
  var child2 = new Child('xiaofeng', 18);
  child1.colors.push('black');
  console.log(child1.colors) // ["red", "blue", "yellow", "black"]
  console.log(child2.colors) //["red", "blue", "yellow"]
  console.log(child1.showName()) // longzi
  ```
* ES6: extends
  ```
  class Parent {
  }
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

> * 参考：[简书——Javascript 的继承与多态](https://www.jianshu.com/p/5cb692658704)
> * 参考：[Dunizb——JS 继承方式总结](https://mp.weixin.qq.com/s?__biz=MzI0MDIwNTQ1Mg==&mid=2676491918&idx=1&sn=2a30b02356595e974537c78b2a82f8eb&chksm=f362cd6dc415447b96cd97db4857b40146ee1b8ba1191ecffdb30644152c6386373a2b5717c1#rd)
> * 参考：[TG——JavaScript 实现继承的方式](http://ghmagical.com/article/page/id/omIoPb1AIBPu)
> * 参考：[ECMAScript 6 入门——class 的继承](http://es6.ruanyifeng.com/#docs/class-extends)

#### new 内部的原理

```
var a = new myFunction("Li","Cherry");
new myFunction{
    var obj = {}; // 创建一个空对象 obj
    obj.__proto__ = myFunction.prototype; // 将新创建的空对象的隐式原型指向其构造函数的显示原型
    var result = myFunction.call(obj,"Li","Cherry"); // 使用 call 改变 this 的指向
    return typeof result === 'object'? result : obj; // 如果无返回值或者返回一个非对象值，则将 obj 返回作为新对象；如果返回值是一个新对象的话那么直接直接返回该对象
}
```

#### instanceof / typeof

* 例子
  ```
  let s = new String('abc');
  typeof s // object
  s instanceof String // true
  typeof null // object
  null instanceof null // TypeError: Right-hand side of 'instanceof' is not an object
  function Foo() {}
  Foo instanceof Object // true
  Foo instanceof Function // true
  ```
* typeof  
   typeof 一般用来判断 number, string, object, boolean, function, undefined, symbol
* instanceof
  * 主要的作用就是判断一个实例是否属于某种类型
  * 也可以判断一个实例是否是其父类型或者祖先类型的实例
    ```
    const person = function () {}
    const programmer = function () {}
    programmer.prototype = new person()
    let nicole = new programmer()
    nicole instanceof person // true
    nicole instanceof programmer // true
    ```

#### this、call、apply、bind

* 下面的文章写得好，多看几遍，懒得总结了
* 参考：[掘金——this、apply、call、bind](https://juejin.im/post/59bfe84351882531b730bac2)
* 参考：[掘金——this（他喵的）到底是什么 — 理解 JavaScript 中的 this、call、apply 和 bind](https://juejin.im/post/5b9f176b6fb9a05d3827d03f)

#### 作用域

* 作用域链  
   在查找变量的时候，先在函数作用域中查找，没有找到，再去全局作用域中查找，有一个从里往外查找的过程。
  ![image](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/F1866E20E2F34E769495F1BEF73D6803/4723)
* 词法作用域(静态作用域)  
   JavaScript 采用的是词法作用域，函数的作用域在函数定义的时候就决定了，即在书写代码时就确定了
  ```
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
* 动态作用域  
   动态作用域与词法作用域相反，函数的作用域是在函数调用的时候才决定的。  
   上面代码如果是动态作用域，则会输出 2

#### 闭包

* 概念：闭包是一个可以访问它外部函数作用域的一个函数，即使这个外部函数已经返回了。
* 闭包是一种现象，不用特意创建，有时候不知不觉就写出来了
* 函数的作用域及其所有变量都会在函数执行结束后被销毁。但是，在创建了一个闭包以后，这个函数的作用域就会一直保存到闭包不存在为止。
* 缺陷：缺点就是常驻内存会增大内存使用量，并且使用不当很容易造成内存泄露。
* 运用闭包的关键：
  * 闭包引用外部函数变量对象中的值
  * 在外部函数的外部调用闭包。
* 例子 1：
  ```
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
* 例子 2：
  ```
  function getCounter() {
    let counter = 0;
    return function() {
      return counter++;
    }
  }
  let count = getCounter();
  console.log(count());  // 0
  console.log(count());  // 1
  console.log(count());  // 2
  // 调用getCounter函数返回一个匿名内部函数，保存到count变量中。此时count函数现在是一个闭包，可以在即使在getCounter函数返回后访问getCounter函数的变量couneter。
  // 但是请注意，counter的值在每次count函数调用时都不会像通常那样重置为0。因为每次调用count()的时候，都会创建新的函数作用域，但是只为getCounter函数创建一个作用域，因为变量counter定义在getCounter函数作用域内，所以每次调用count函数时数值会增加而不是重置为0。
  ```
* 常见闭包面试题

  ```
    for (var i = 1; i < 10; i++) {
    	setTimeout(function () {
    		console.log(i);
    	}, 1000);
    } // 10 10 10 10 10 10 10 10 10 10

    for (let i = 1; i < 10; i++) {
    	setTimeout(function () {
    		console.log(i);
    	}, 1000);
    } // 1 2 3 4 5 6 7 8 9

    for (var i = 1; i <= 10; i++) {
    	(function (j) {
    		setTimeout(function () {
    			console.log(j);
    		}, 1000);
    	})(i);
    }// 1 2 3 4 5 6 7 8 9
  ```

> 参考：[掘金——闭包详解](https://juejin.im/post/5b081f8d6fb9a07a9b3664b6)  
> 参考：[掘金——理解 JavaScript 闭包——新手指南](理解JavaScript闭包——新手指南)

#### 造成内存泄漏的常见情况

* 意外的全局变量
* 定时器未清除
* dom 清空时，还存在引用
* 闭包(好像只有 ie 会)

#### ES6

###### Promise

* resolve 函数在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
* reject 函数，在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
* Promise.prototype.finally()：用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。
  ```
  promise
  .then(result => {···})
  .catch(error => {···})
  .finally(() => {···});
  ```
* Promise.all()
  * 用于将多个 Promise 实例，包装成一个新的 Promise 实例
  * 接受一个数组作为参数，p1、p2、p3 都是 Promise 实例，如果不是，就会先调用 Promise.resolve 方法，将参数转为 Promise 实例
  * Promise.all 方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
    ```
    const p = Promise.all([p1, p2, p3]);
    // 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
    // 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
    ```
* Promise.race()
  * 基本语法与 Promise.all()类似
  * 返回的规则不同
    ```
    const p = Promise.race([p1, p2, p3]);
    // 只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。
    // 率先改变的 Promise 实例的返回值，就传递给p的回调函数
    ```
* Promise.resolve(): 将现有对象转为 Promise 对象
* Promise.reject(reason): 返回一个新的 Promise 实例，该实例的状态为 rejected

  ```
  const p = Promise.reject('出错了');
  // 等同于
  const p = new Promise((resolve, reject) => reject('出错了'))

  p.then(null, function (s) {
    console.log(s)
  });
  // 出错了
  ```

###### async await

* 是 Generator 函数的语法糖
* 与 Generator 的区别
  * 内置执行器：Generator 函数的执行必须靠执行器，async 函数的执行，与普通函数一模一样，只要一行
  * 更好的语义
  * Generator 函数的返回值是 Iterator 对象，async 函数的返回值是 Promise 对象
* 遇到 await 表达式时，会让 async 函数 暂停执行，等到 await 后面的语句（Promise）状态发生改变（resolved 或者 rejected）之后，再恢复 async 函数的执行（再之后 await 下面的语句），并返回解析值（Promise 的值）
* 例子

  ```
    async function async1(){
        console.log('async1 start')
        await async2()
        console.log('async1 end')
    }
    async function async2(){
        console.log('async2')
    }
    console.log('script start')
    setTimeout(function(){
        console.log('setTimeout')
    },0)  
    async1();
    new Promise(function(resolve){
        console.log('promise1')
        resolve();
    }).then(function(){
        console.log('promise2')
    })
    console.log('script end')

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

* Generator 是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
* 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，必须调用遍历器对象的 next 方法，使得指针移向下一个状态
* 换言之，Generator 函数是分段执行的，yield 表达式是暂停执行的标记，而 next 方法可以恢复执行

  ```
  function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
  }

  var hw = helloWorldGenerator();
  hw.next() // { value: 'hello', done: false }
  hw.next() // { value: 'world', done: false }
  hw.next() // { value: 'ending', done: true }
  hw.next() // { value: undefined, done: true }
  ```

* Generator 函数可以不用 yield 表达式，这时就变成了一个单纯的暂缓执行函数。
* 语法：下面四种都行
  ```
  function * foo(x, y) { ··· }
  function *foo(x, y) { ··· }
  function* foo(x, y) { ··· }
  function*foo(x, y) { ··· }
  ```
* yield 表达式
  * 遇到 yield 表达式，就暂停执行后面的操作，并将紧跟在 yield 后面的那个表达式的值，作为返回的对象的 value 属性值
  * 下一次调用 next 方法时，再继续往下执行，直到遇到下一个 yield 表达式。
  * 如果没有再遇到新的 yield 表达式，就一直运行到函数结束，直到 return 语句为止，并将 return 语句后面的表达式的值，作为返回的对象的 value 属性值。
  * 如果该函数没有 return 语句，则返回的对象的 value 属性值为 undefined。
* yield\*表达式

  * 在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的
  * yield\*表达式，用来在一个 Generator 函数里面执行另一个 Generator 函数

    ```
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

    for (let v of bar()){
      console.log(v);
    }
    // "x"
    // "a"
    // "b"
    // "y"
    ```

* for...of 循环

  * for...of 循环可以自动遍历 Generator 函数运行时生成的 Iterator 对象，且此时不再需要调用 next 方法。

    ```
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

* Set

  * 类似于数组，但是成员的值都是唯一的，没有重复的值
  * Set 加入值的时候，不会发生类型转换，5 和"5"是两个不同的值
  * Set 内部, 两个 NaN 是相等
  * Set 本身是一个构造函数，用来生成 Set 数据结构。
  * Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数

    ```
    const s = new Set();
    [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
    for (let i of s) {
      console.log(i);
    } // 2 3 5 4

    // 字符串去重
    [...new Set('ababbc')].join('') //"abc"
    ```

  * Array.from 方法可以将 Set 结构转为数组。
    ```
    const items = new Set([1, 2, 3, 4, 5]);
    const array = Array.from(items);
    console.log(array) // [1, 2, 3, 4, 5]
    ```
  * 使用 Set 可以很容易地实现并集、交集和差集

    ```
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);

    // 并集
    let union = new Set([...a, ...b]);
    // Set {1, 2, 3, 4}

    // 交集
    let intersect = new Set([...a].filter(x => b.has(x)));
    // set {2, 3}

    // 差集
    let difference = new Set([...a].filter(x => !b.has(x)));
    // Set {1}
    ```

  * 实例属性：Set.prototype.size：返回 Set 实例的成员总数
  * 实例方法
    * add(value)：添加某个值，返回 Set 结构本身。
    * delete(value)：删除某个值，返回一个布尔值
    * has(value)：返回一个布尔值，表示该值是否为 Set 的成员。
    * clear()：清除所有成员，没有返回值。

* WeakSet: (尽量记一下)
  * WeakSet 的成员只能是对象，而不能是其他类型的值
  * WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中
* Map

  * 它类似于对象，也是键值对的集合
  * 但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键
  * 实例属性：size：属性返回 Map 结构的成员总数。
  * 实例方法

    * set(key, value)：set 方法设置键名 key 对应的键值为 value，然后返回整个 Map 结构

      ```
      const m = new Map();

      m.set('edition', 6)        // 键是字符串
      m.set(262, 'standard')     // 键是数值
      m.set(undefined, 'nah')    // 键是 undefined
      ```

    * get(key):读取 key 对应的键值，如果找不到 key，返回 undefined。
    * delete(key):删除某个键，返回一个布尔值
    * has(value)：返回一个布尔值，表示某个键是否在当前 Map 对象之中。
    * clear()：清除所有成员，没有返回值。

* WeakMap
  * WeakMap 只接受对象作为键名（null 除外），不接受其他类型的值作为键名
  * WeakMap 的键名所指向的对象，不计入垃圾回收机制。

###### Proxy

* 在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截
* ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例
  `var proxy = new Proxy(target, handler);`
  > 能记这些就差不多了，再问直接放弃

###### Symbol

* 一种新的原始数据类型 Symbol，表示独一无二的值
* Symbol 函数前不能使用 new 命令，否则会报错
* Symbol 函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

  ```
    let s = Symbol();
    typeof s // "symbol"

    let s1 = Symbol('foo');
    let s2 = Symbol('bar');
    s1 // Symbol(foo)
    s2 // Symbol(bar)
    s1.toString() // "Symbol(foo)"
    s2.toString() // "Symbol(bar)"
  ```

  > 先记这么多，脑子不够用了

###### Interator / for of

* 原生具备 Iterator 接口的数据结构如下。
  * Array
  * Map
  * Set
  * String
  * TypedArray
  * 函数的 arguments 对象
  * NodeList 对象
* for...of
  * for...of 循环内部调用的是数据结构的 Symbol.iterator 方法
  * 一个数据结构只要部署了 Symbol.iterator 属性，就被视为具有 iterator 接口，就可以用 for...of 循环遍历它的成员。
* 其他调用 Iterator 接口的场合
  * 解构赋值：对数组和 Set 结构进行解构赋值时
  * 扩展运算符（...）
  * Array.from()
  * yield\*
  * Promise.all() / Promise.race()

###### Object 扩展

* Object.assign
  * 第一个参数是目标对象，后面的参数都是源对象，返回目标对象
  * 如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
  * 只拷贝源对象的自身属性（不拷贝继承属性）
  * 也不拷贝不可枚举的属性（enumerable: false）
* Object.setPrototypeOf()
  * 作用与**proto**相同，用来设置一个对象的 prototype 对象，返回参数对象本身
  * 语法：Object.setPrototypeOf(object, prototype)
  * 等同于
    ```
    function setPrototypeOf(obj, proto) {
      obj.__proto__ = proto;
      return obj;
    }
    ```
* Object.getPrototypeOf()
  * 与 Object.setPrototypeOf 方法配套，用于读取一个对象的原型对象
  * 语法：Object.getPrototypeOf(obj);
* Object.keys()
  * ES5 引入
  * 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。
  * 例子：
    ```
    var obj = { foo: 'bar', baz: 42 };
    Object.keys(obj)
    // ["foo", "baz"]
    ```
* Object.values()
  * 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。
  * 只返回对象自身的可遍历属性
  * 例子：
    ```
    const obj = { foo: 'bar', baz: 42 };
    Object.values(obj)
    // ["bar", 42]
    ```
* Object.entries()
  * 方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组
  * 例子
    ```
    const obj = { foo: 'bar', baz: 42 };
    Object.entries(obj)
    // [ ["foo", "bar"], ["baz", 42] ]
    ```

###### Array 扩展

* Array.from()
  * 只要是部署了 Iterator 接口的数据结构，Array.from 都能将其转为数组
  * Array.from 还可以接受第二个参数，作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
* Array.of()
  * 用于将一组值，转换为数组
  * 这个方法的主要目的，是弥补数组构造函数 Array()的不足。因为参数个数的不同，会导致 Array()的行为有差异。
    ```
    Array() // []
    Array(3) // [, , ,]
    Array(3, 11, 8) // [3, 11, 8]
    ```
  * 例子：
    ```
    Array.of(3, 11, 8) // [3,11,8]
    Array.of(3) // [3]
    Array.of(3).length // 1
    ```
* 数组实例的 find() 和 findIndex()
  * find 方法，用于找出第一个符合条件的数组成员。
  * 参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为 true 的成员，然后返回该成员。
  * 如果没有符合条件的成员，则返回 undefined。
    ```
    [1, 5, 10, 15].find(function(value, index, arr) {
      return value > 9;
    }) // 10
    ```
  * findIndex 方法的用法与 find 方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。
* 数组实例的 fill()
  * 主要用于空数组的初始化非常方便
    ```
    new Array(3).fill(7) // [7, 7, 7]
    ```
* 数组实例的 includes()
  * 返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。
    ```
    [1, 2, 3].includes(2)     // true
    [1, 2, 3].includes(4)     // false
    [1, 2, NaN].includes(NaN) // true
    ```
  * Map 和 Set 数据结构有一个 has 方法, 需要注意与 includes 区分
    * Map 结构的 has 方法，是用来查找键名的
    * Set 结构的 has 方法，是用来查找值的
* 数组实例的 flat()，flatMap()
  * 用于将嵌套的数组“拉平”，变成一维的数组。
  * 返回一个新数组，对原数据没有影响。
  * 默认只会“拉平”一层
    ```
    [1, 2, [3, [4, 5]]].flat()  // [1, 2, 3, [4, 5]]
    [1, 2, [3, [4, 5]]].flat(2)  // [1, 2, 3, 4, 5]
    [1, [2, [3]]].flat(Infinity) // [1, 2, 3]
    ```
  * flatMap()方法对原数组的每个成员执行一个函数（相当于执行 map()），然后对返回值组成的数组执行 flat()方法
  * flatMap()只能展开一层数组
    ```
    // 相当于 [[2, 4], [3, 6], [4, 8]].flat()
    [2, 3, 4].flatMap((x) => [x, x * 2])
    // [2, 4, 3, 6, 4, 8]
    ```

#### Array

* forEach map reduce filter some every
  ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/B80B41925B7D4DAA92EB2A0A536DB6A9/4801)

> 参考 [掘金——数组迭代方法图解](https://juejin.im/post/5835808067f3560065ed4ab2)

* reduce todo
* slice splice todo

#### for in / for of todo

#### 事件循环(EventLoop)

![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/CDE1E05AA6924122A3BC9793CF2C6D0E/5042)

* 同步和异步任务分别进入不同的执行"场所"
* 同步的进入主线程，异步的进入 Event Table 并注册函数。
* 当指定的事情完成时，Event Table 会将这个函数移入 Event Queue。
* 主线程内的任务执行完毕为空，会去 Event Queue 读取对应的函数，进入主线程执行。
* 上述过程会不断重复，也就是常说的 Event Loop(事件循环)。

###### 宏任务，微任务

![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/AC64F1B26FF04EB5A7724F4A8E2080F9/5052)

* 例子

  ```
  console.log('1');

  setTimeout(function() {
      console.log('2');
      process.nextTick(function() {
          console.log('3');
      })
      new Promise(function(resolve) {
          console.log('4');
          resolve();
      }).then(function() {
          console.log('5')
      })
  }, 500)
  process.nextTick(function() {
      console.log('6');
  })
  new Promise(function(resolve) {
      console.log('7');
      resolve();
  }).then(function() {
      console.log('8')
  })

  setTimeout(function() {
      console.log('9');
      process.nextTick(function() {
          console.log('10');
      })
      new Promise(function(resolve) {
          console.log('11');
          resolve();
      }).then(function() {
          console.log('12')
      })
  }, 1000)
  // 1，7，6，8，2，4，3，5，9，11，10，12
  ```

  > 写的比较简略，详情请看参考:[掘金——这一次，彻底弄懂 JavaScript 执行机制](https://segmentfault.com/a/1190000018227028)

#### 递归 todo

#### 浅拷贝 深拷贝

* 数据分为基本数据类型(String,Number,Boolean,Null,Undefined)和引用数据类型(Object,Array,Function)
* 对于基本数据类型，浅拷贝深拷贝都是拷贝值，但对于引用数据类型，他们有以下区别
  * 浅拷贝只复制某个对象的地址，新旧对象还是共享同一块内存
  * 深拷贝会开辟一块新的内存空间，修改新对象不会改到原对象
* 浅拷贝和赋值的区别

  | type   | 和原数据是否指向同一对象 | 第一层数据为基本数据类型 | 原数据中包含子对象       |
  | ------ | ------------------------ | ------------------------ | ------------------------ |
  | 赋值   | 是                       | 改变会使原数据一同改变   | 改变会使原数据一同改变   |
  | 浅拷贝 | 否                       | 改变不会使原数据一同改变 | 改变会使原数据一同改变   |
  | 深拷贝 | 否                       | 改变不会使原数据一同改变 | 改变不会使原数据一同改变 |

* 常见浅拷贝方法
  * 扩展运算符 ...
  * Array.prototype.concat()
  * Object.assign() `当Object只有一层的时候是深拷贝`
* 常见深拷贝方法
  * JSON.parse(JSON.stringify()) `但是JSON.stringify无法处理函数`
  * lodash 方法 `lodash.cloneDeep()`
  * 利用递归实现

```
const deepClone = obj => {
  let clone = Object.assign({}, obj);
  Object.keys(clone).forEach(
    key => (clone[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key])
  );
  return Array.isArray(obj) ? (clone.length = obj.length) && Array.from(clone) : clone;
};
```

> * 参考：[知乎——javascript 中的深拷贝和浅拷贝？](https://www.zhihu.com/question/23031215)
> * 参考：[掘金——浅拷贝与深拷贝](https://juejin.im/post/5b5dcf8351882519790c9a2e)
> * 参考：[掘金——深拷贝 vs 浅拷贝](https://juejin.im/post/59ac1c4ef265da248e75892b)

#### 防抖、节流

* 防抖和节流的作用都是防止函数多次调用
* 防抖：任务频繁触发的情况下，只有任务触发的间隔超过指定间隔的时候，任务才会执行。
  * 应用场景(执行最后一次)
    * input 在 onchange 时候的触发(用户输入验证/搜索等)
    * 按钮重复点击(也是一种场景，不过我习惯用 btnLoading)
  * 代码实现
    ```
    // 简单实现
    const debounce = (fn, ms = 0) => {
      let timeoutId;
      return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => fn.apply(this, args), ms);
      };
    };
    // 参考二有更好的方式
    ```
* 节流：指定时间间隔内只会执行一次任务； - 应用场景(有间隔地持续执行) - 鼠标滚动时会执行的函数 - 窗口 resize - 拖拽(slider) - 代码实现
  ```
  const throttle = (fn, wait) => {
    let inThrottle, lastFn, lastTime;
    return function() {
      const context = this,
        args = arguments;
      if (!inThrottle) {
        fn.apply(context, args);
        lastTime = Date.now();
        inThrottle = true;
      } else {
        clearTimeout(lastFn);
        lastFn = setTimeout(function() {
          if (Date.now() - lastTime >= wait) {
            fn.apply(context, args);
            lastTime = Date.now();
          }
        }, Math.max(wait - (Date.now() - lastTime), 0));
      }
    };
  };
  ```

> * 参考：[掘金——函数节流与函数防抖](https://juejin.im/entry/58c0379e44d9040068dc952f)
> * 参考：[InterviewMap——防抖](https://yuchengkai.cn/docs/frontend/#%E9%98%B2%E6%8A%96)
> * 参考：[github issues——节流和防抖的个人见解](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/5)

#### 柯里化

* 是高阶函数的一种特殊用法
* 定义：接收函数 A 作为参数，运行后能够返回一个新的函数，并且这个新的函数能够处理函数 A 的剩余参数。
* 实现
  * lodash.curry()
  * 简单实现，参数只能从右到左传递
    ```
    function createCurry(func, args) {
        var arity = func.length;
        var args = args || [];
        return function() {
            var _args = [].slice.call(arguments);
            [].push.apply(_args, args);
            // 如果参数个数小于最初的func.length，则递归调用，继续收集参数
            if (_args.length < arity) {
                return createCurry.call(this, func, _args);
            }
            // 参数收集完毕，则执行func
            return func.apply(this, _args);
        }
    }
    ```
  * 30 seconds of code:
    ```
    const curry = (fn, arity = fn.length, ...args) =>
      arity <= args.length ? fn(...args) : curry.bind(null, fn, arity, ...args);
    ```
* 例子
  ```
  function add(a, b, c) {
      return a + b + c;
  }
  createCurry(add)(1,2,3) // 6
  createCurry(add)(1,2)(3) // 6
  createCurry(add)(1)(2)(3) // 6
  curry(add)(1)(2)(3) // 6
  curry(add)(1)(2,3) // 6
  ```
  > * 参考：[llh911001——JS 函数式编程指南](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html#%E4%B8%8D%E4%BB%85%E4%BB%85%E6%98%AF%E5%8F%8C%E5%85%B3%E8%AF%AD%E5%92%96%E5%96%B1)
  > * 参考：[简书——深入详解函数的柯里化](https://www.jianshu.com/p/5e1899fe7d6b)
  > * 参考：[segmentfault——简述几个非常有用的柯里化函数使用场景](https://segmentfault.com/a/1190000015281061)

## React

#### 生命周期

* 创建阶段
  * constructor
  * static getDerivedStateFromProps
  * render
  * componentDidMount
* 更新阶段
  * static getDerivedStateFromProps
  * shouldComponentUpdate
  * render
  * getSnapshotBeforeUpdate
  * componentDidUpdate
* 卸载阶段 - componentWillUnmount
  > * componentWillMount、componentWillUpdate、componentWillReceiveProps 是遗留的方法，在 v17.\*\*大版本中就不能用了
  > * getDerivedStateFromProps，用的时候要比较注意。[官网——You Probably Don't Need Derived State](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)
  > * 参考：[官网——React.Component](https://reactjs.org/docs/react-component.html)
  > * 参考：[官网——生命周期图](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

#### 组件之间怎么通信

* 父级传给子级：this.props
* 子级传给父级：ref
* 跨组件传递：context，redux

#### react 中 key 的作用是什么

* 为了在 diff 算法执行时更快的找到对应的节点，提高 diff 速度
* key 变化的时候，节点会重新渲染
* map 的时候尽量不要用 index 做 key 值
  > 举例： 如果希望可以在 B 和 C 之间加一个 F
  > Diff 算法默认执行起来是这样的
  > ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/89713889899F496AB5A92E2A52B8A163/5375)
  > 即把 C 更新成 F，D 更新成 C，E 更新成 D，最后再插入 E，是不是很没有效率？<br />
  > 所以可是使用 key 来给每个节点做一个唯一标识，Diff 算法就可以正确的识别此节点，找到正确的位置区插入新的节点。<br /> > ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/8E6617DF1C4E47D480CF5B377EF2A863/5403)

#### diff 算法

* render 执行的结果得到的不是真正的 DOM 节点，仅仅是轻量级的 JavaScript 对象, 我们称之为 virtual DOM.

> 内容太多了，需要深究，被问到就先答一下 key 的重要性
>
> * 参考：[掘金——浅谈 React 中的 diff](https://juejin.im/post/5ac355576fb9a028cc616aad)
> * 参考：[segmentfault——React 的 diff 算法](https://segmentfault.com/a/1190000000606216)

#### function Component / class Component todo

#### Controlled Component / Uncontrolled Component todo

#### ref todo

#### react hooks todo
#### react-redux todo

#### 组件的复用

* High order component
* render props
* custom hooks

## RN todo

## 浏览器

#### session、cookie、localstorage、sessionStorage、IndexDB

* cookie：保存在浏览器端， 大小 4K，可以设置过期时间
* session：保存在服务器端，无大小限制
* localstorage：除非手动清除，否则永久保存，大小 5M
* sessionStorage：与 localStorage 类似，不过浏览器关闭时会自动清空
  ![](https://user-gold-cdn.xitu.io/2018/12/14/167ac245e669b3b3?imageslim)
* IndexDB: 支持的浏览器比较广泛，大小限制通常约为 50MB。

#### 输入一个网址到页面展示，发生了什么事情

* DNS 解析:将域名解析成 IP 地址
* TCP 连接：TCP 三次握手
* 发送 HTTP 请求
* 服务器处理请求(期间可能会读取服务器缓存或查询数据库)，并返回响应报文
* 浏览器根据返回的状态码判断是下载还是从缓存读取文件内容
* 浏览器解析渲染页面
* 断开连接：TCP 四次挥手
  > * 参考：[掘金——浏览器基础](https://juejin.im/post/5c137e7c6fb9a049f7461639#heading-2)
  > * 参考：[掘金——从 URL 输入到页面展现到底发生什么？](https://juejin.im/post/5bf3ad55f265da61682afc9b)

#### TCP 三次握手 四次挥手

* 三次握手
  * 客户端发送 SYN 报文
  * 服务端发送 SYN+ACK 报文
  * 客户端再发送 ACK 确认包
* 四次挥手 - 主动方发送 FIN 报文 - 接收方收到 FIN，发回一个 ACK 确认 - 接收方也发送一个 FIN - 主动方收到后发回 ACK 确认
  > * 参考：动画讲解：[掘金——三次握手 四次挥手](https://juejin.im/post/5b29d2c4e51d4558b80b1d8c)
  > * 参考：[博客园——为什么不是 2 次握手或者其他次](https://www.cnblogs.com/zhuzhenwei918/p/7465467.html)

#### 常见状态码

* 1xx 信息相关
* 2XX 成功
  * 200 OK，成功
  * 204 No content，表示请求成功，但没有返回任何内容
* 3XX 重定向
  * 301 moved permanently，永久性重定向，表示资源已被分配了新的 URL
  * 302 found，临时性重定向，表示资源临时被分配了新的 URL
  * 304 not modified，表示未修改，读取缓存
* 4XX 客户端错误
  * 400 bad request，请求报文存在语法错误
  * 401 unauthorized，需要身份验证
  * 403 forbidden，表示对请求资源的访问被服务器拒绝
  * 404 not found，表示在服务器上没有找到请求的资源
* 5XX 服务器错误 - 500 internal sever error，表示服务器端在执行请求时发生了错误 - 503 service unavailable，服务不可用
  > * 记住上面这些面试应该够了，工作需要其他的直接去 google 吧~

#### http 请求头里都有什么内容 todo

#### http 和 https 的区别

* HTTPS 协议需要到 CA 申请证书，一般免费证书很少，需要交费
* HTTP 协议运行在 TCP 之上，所有传输的内容都是明文，HTTPS 运行在 SSL/TLS 之上，SSL/TLS 运行在 TCP 之上，所有传输的内容都经过加密的。
* HTTP 和 HTTPS 端口也不一样，前者是 80，后者是 443。
* HTTPS 可以有效的防止运营商劫持
* HTTPS比HTTP更加安全，对搜索引擎更友好，利于SEO
  
#### http 1.0、http 1.1 和 http 2.0 的区别 
- http1.0
  - 缺陷：浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接（TCP连接的新建成本很高，因为需要客户端和服务器三次握手），服务器完成请求处理后立即断开TCP连接，服务器不跟踪每个客户也不记录过去的请求；
  - 解决方案：添加头信息——非标准的Connection字段Connection: keep-alive
- http1.1
  - 改进点
    - 持久连接：引入了持久连接，即TCP连接默认不关闭，可以被多个请求复用，不用声明Connection: keep-alive(对于同一个域名，大多数浏览器允许同时建立6个持久连接)
    - 管道机制：在同一个TCP连接里面，客户端可以同时发送多个请求
    - 分块传输编码：服务端没产生一块数据，就发送一块，采用”流模式”而取代”缓存模式”。
    - 新增请求方式：put,delete,options,trace,connect
  - 缺点
    - 虽然允许复用TCP连接，但是同一个TCP连接里面，所有的数据通信是按次序进行的。服务器只有处理完一个请求，才会接着处理下一个请求。如果前面的处理特别慢，后面就会有许多请求排队等着。这将导致“队头堵塞”
    - 避免方式：一是减少请求数，二是同时多开持久连接
- http 2.0
  - 二进制协议：HTTP/2 则是一个彻底的二进制协议，头信息和数据体都是二进制，并且统称为”帧”：头信息帧和数据帧。
  - 完全多路复用：HTTP/2 复用TCP连接，在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，而且不用按照顺序一一对应，这样就避免了”队头堵塞”。
  - 报头压缩
  - 服务器推送：允许服务器未经请求，主动向客户端发送资源
- 总结
  |    http1.0     |                                  http1.1                                   |                               http2.0                                |
  | :------------: | :------------------------------------------------------------------------: | :------------------------------------------------------------------: |
  | 无状态、无连接 | 持久连接<br/>请求管道化<br/>加缓存处理<br/>增加Host字段<br/>支持断点传输等 | 二进制分帧 <br/> 多路复用（或连接共享）<br/> 头部压缩<br/>服务器推送 |
> * 参考：[HTTP1.0 HTTP1.1 HTTP2.0 主要特性对比/2](https://segmentfault.com/a/1190000013028798)
> * 参考：[HTTP1.0、HTTP1.1 和 HTTP2.0 的区别](https://juejin.im/entry/6844903489596833800)
> * 参考：[HTTP/2 服务器推送（Server Push）教程](http://www.ruanyifeng.com/blog/2018/03/http2_server_push.html)
> * 参考：[深入理解http1.x、http 2和https](https://segmentfault.com/a/1190000015316332)
> * 参考：[怎样把网站升级到http/2](https://zhuanlan.zhihu.com/p/29609078)

#### http 3.0  
- 2018 年，QUIC 演变成为 HTTP3。
- 占比约5%
- HTTP3 的主要改进在传输层上。不走 TCP 连接了，走 UDP。  

> * 参考：[秒懂科普！HTTP 3 如此简单](https://segmentfault.com/a/1190000023929858)



#### 跨域以及常见解决办法

* 浏览器有一个安全策略叫同源策略，同源就是要求, 域名, 协议, 端口相同，任意一个不同则叫不同域
* 同源策略还会引起 Cookie、LocalStorage 和 IndexDB 无法读取
* 不同域之间 iframe, ajax 均受其限制, script 标签不受此限制.
* 解决跨域的方式

  * 使用代理 - 请求同域下的 web 服务器; - web 服务器像代理一样去请求真正的第三方服务器; - 代理拿到数据过后, 直接返回给客户端 ajax.
  * jsonp: 即 JSON with padding。因为 script 标签是不会跨域的，利用这个特性，可以解决跨域，兼容性很好，但是只支持 GET 方式
    ```
    //需要跨域的时候可以创建一个script标签
    var jsonp = document.createElement("script");
    //指定类型
    jsonp.type = "text/javascript";
    //添加链接地址
    jsonp.src = "http://10.0.154.249/text.js?callback=jsonpCallback";
    //将这个拼接 添加到head标签中。
    document.getElementsByTagName("head")[0].appendChild(jsonp);
    //回调函数
    function jsonpCallback(ret){
        alert(ret)
    }
    ```
  * CORS：原理是使用自定义的 HTTP 头部，让服务器与浏览器进行沟通，主要是通过设置响应头的 Access-Control-Allow-Origin: \* 来达到目的
  * postMessage：postMessage(data,origin)
    ```
    // 父页面发送消息
    window.frames[0].postMessage('message', origin)
    // 子页面监听自身的message事件来获取传过来的消息
    window.addEventListener('message',function(e){
        if(e.source!=window.parent) return;//若消息源不是父页面则退出
          //TODO ...
    });
    ```
  * window.name：在一个 window 的生命周期内,窗口载入的所有的页面都是共享一个 window.name 的，即当 window 的 location 变化并且重新加载,它的 name 属性可以依然保持不变.(还是要借助 iframe)
  * document.domain：不同的子域 + iframe 交互的时候，可以借助 document.domain 来解决。不过 document.domain 只能设置为页面本身或者更高一级的域名。 - location.hash：利用 url 的 hash，因为服务端不关心这部分。
    > * 有些不常用的，看不太懂，硬背下来吧
    > * 参考：[掘金——由同源策略到前端跨域](https://juejin.im/post/58f816198d6d81005874fd97)
    > * 参考：[segmentfault——浅谈浏览器端 JavaScript 跨域解决方法](https://segmentfault.com/a/1190000004518374)
    > * 参考：[简书——浏览器中使用 js 跨域获取数据的几种方式](https://www.jianshu.com/p/c71c20e98f94)

## 其他

#### 对前端模块化的理解 todo

#### CI/CD todo
