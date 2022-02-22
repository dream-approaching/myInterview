<!-- TOC -->

- [1. html + css](#1-html--css)
    - [1.1. 用 border 绘制三角形](#11-用-border-绘制三角形)
    - [1.2. 修改 placeholder 样式](#12-修改-placeholder-样式)
    - [1.3. 自定义滚动条](#13-自定义滚动条)
    - [1.4. 隐藏滚动条](#14-隐藏滚动条)
    - [1.5. 超出省略号](#15-超出省略号)
    - [1.6. 固定行数超出省略号](#16-固定行数超出省略号)
    - [1.7. css 盒模型](#17-css-盒模型)
    - [1.8. css3 动画样式](#18-css3-动画样式)
    - [1.9. css 变量 / css 自定义属性](#19-css-变量--css-自定义属性)
    - [1.10. BFC](#110-bfc)
    - [1.11. 垂直居中](#111-垂直居中)

<!-- /TOC -->
# 1. html + css

## 1.1. 用 border 绘制三角形

```css
width: 0;
height: 0;
border: 30px solid;
border-color: transparent transparent lightblue;
```

> 参考： [简书——CSS 绘制三角形](https://www.jianshu.com/p/9a463d50e441)

## 1.2. 修改 placeholder 样式

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

## 1.3. 自定义滚动条

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

## 1.4. 隐藏滚动条

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

## 1.5. 超出省略号

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: nowrap;
width: 160px;
```

## 1.6. 固定行数超出省略号

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

## 1.7. css 盒模型

- box-sizing: content-box（W3C 盒模型，又名标准盒模型）：元素的宽高大小表现为内容的大小。
- box-sizing: border-box（IE 盒模型，又名怪异盒模型）：元素的宽高表现为内容 + 内边距 + 边框的大小。背景会延伸到边框的外沿。

> 若不声明 DOCTYPE 类型，IE 浏览器会将盒子模型解释为 IE 盒子模型，FireFox 等会将其解释为 W3C 盒子模型；若在页面中声明了 DOCTYPE 类型，所有的浏览器都会把盒模型解释为 W3C 盒模型。  
> html5 中写`<!DOCTYPE html>`

## 1.8. css3 动画样式

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

## 1.9. css 变量 / css 自定义属性

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

## 1.10. BFC

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

## 1.11. 垂直居中

- line-height 等于 hieght
- vertical-align: middle 需要是 inline-block
- 绝对定位 `top:50%` 配合 `margin-top: -height / 2` 或者 `transform:translateY(-50%);`(优点是不需要知道宽高)
- 绝对定位 top、right、bottom、left 都为 0 配合 margin: auto;
- flex 布局