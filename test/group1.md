<!-- TOC -->

- [1. Group 1](#1-group-1)
    - [1.1. 如何解决 a 标签点击后 hover 事件失效的问题?](#11-如何解决-a-标签点击后-hover-事件失效的问题)
    - [1.2. 常见手写函数](#12-常见手写函数)
        - [1.2.1. 闭包](#121-闭包)
        - [1.2.2. 柯里化](#122-柯里化)
        - [1.2.3. 防抖](#123-防抖)
        - [1.2.4. 节流](#124-节流)
        - [1.2.5. 深拷贝](#125-深拷贝)
        - [1.2.6. 继承](#126-继承)
        - [1.2.7. new 内部原理](#127-new-内部原理)
        - [1.2.8. 实现一个 ajax](#128-实现一个-ajax)
    - [1.3. 点击一个 input 依次触发的事件](#13-点击一个-input-依次触发的事件)
    - [1.4. Vue/React 中 hash 模式和 history 模式的区别](#14-vuereact-中-hash-模式和-history-模式的区别)
    - [1.5. 数组去重](#15-数组去重)
    - [1.6. addEventListener 函数的第三个参数](#16-addeventlistener-函数的第三个参数)
    - [1.7. 所有的事件都有冒泡吗？](#17-所有的事件都有冒泡吗)
    - [1.8. typeof 和 instanceof 的区别](#18-typeof-和-instanceof-的区别)
    - [1.9. 在移动端中怎样初始化根元素的字体大小](#19-在移动端中怎样初始化根元素的字体大小)
    - [1.10. 移动端布局](#110-移动端布局)

<!-- /TOC -->
# 1. Group 1
## 1.1. 如何解决 a 标签点击后 hover 事件失效的问题?

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

## 1.2. 常见手写函数

### 1.2.1. 闭包

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

### 1.2.2. 柯里化

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

### 1.2.3. 防抖

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

### 1.2.4. 节流

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

### 1.2.5. 深拷贝

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

### 1.2.6. 继承

```js
function Parent() {}
function Child(...args) {
  Parent.apply(this, args);
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
```

### 1.2.7. new 内部原理

```js
var a = new myFunction("Li","Cherry");
new MyFunction {
  const obj = {}
  obj.__proto__ = MyFunction.prototype;
  const result = MyFunction.call(obj, "Li","Cherry")
  return typeof result === 'object' ? result : obj;
}
```

### 1.2.8. 实现一个 ajax

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

## 1.3. 点击一个 input 依次触发的事件

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

## 1.4. Vue/React 中 hash 模式和 history 模式的区别

- 最明显的是在显示上，hash 模式的 URL 中会夹杂着#号，而 history 没有。
- hash 模式是依靠 onhashchange 事件(监听 location.hash 的改变)，而 history 模式是主要是依靠的 HTML5 history 的方法
- 这时候 history 模式需要后端的支持。因为 history 模式下，前端的 URL 必须和实际向后端发送请求的 URL 一致。所以需要后端增加一个覆盖所有情况的候选资源，一般会配合前端给出的一个 404 页面。

## 1.5. 数组去重

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


## 1.6. addEventListener 函数的第三个参数

第三个参数涉及到冒泡和捕获，是 true 时为捕获，是 false 则为冒泡 默认值。

或者是一个对象{passive: true}，针对的是 Safari 浏览器，禁止/开启使用滚动的时候要用到。

## 1.7. 所有的事件都有冒泡吗？

并不是所有的事件都有冒泡的，例如以下事件就没有：

- onblur
- onfocus
- onmouseenter
- onmouseleave

## 1.8. typeof 和 instanceof 的区别

typeof 表示是对某个变量类型的检测，基本数据类型除了 null 都能正常的显示为对应的类型，引用类型除了函数会显示为'function'，其它都显示为 object。

instanceof 主要是用于实例的判断。 `A instanceof B` 用来判断 A 是否为 B 的实例。也可以判断一个实例是否是其父类型或者祖先类型的实例

> 扩展：变量类型

- 基本类型： string, number, boolean, undefined, null, Symbol, bigInt(es10 新增)
- 引用类型： function, array, object

## 1.9. 在移动端中怎样初始化根元素的字体大小

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

## 1.10. 移动端布局

- 移动端布局的方式主要使用 rem 和 flex，可以结合 rem 和媒体查询，然后不同的上视口大小下设置设置 html 的 font-size。
- 可单独制作移动端页面也可响应式 pc 端移动端共用一个页面。没有好坏，视情况而定，因势利导
- 我一般是结合两个库`postcss-pxtorem`和`lib-flexible`，前者用于转换单位，后者用于修改根节点字体大小