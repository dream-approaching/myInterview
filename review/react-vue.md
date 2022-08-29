<!-- TOC -->

- [1. React](#1-react)
    - [1.1. 生命周期](#11-生命周期)
    - [1.2. 组件之间怎么通信](#12-组件之间怎么通信)
    - [1.3. react 中 key 的作用是什么](#13-react-中-key-的作用是什么)
    - [1.4. diff 算法](#14-diff-算法)
    - [1.5. function Component / class Component todo](#15-function-component--class-component-todo)
    - [1.6. context](#16-context)
    - [1.7. hooks](#17-hooks)
        - [1.7.1. 优点](#171-优点)
        - [1.7.2. 常用hooks](#172-常用hooks)
    - [1.8. redux](#18-redux)
        - [1.8.1. 使用 redux 时，常用到的 api](#181-使用-redux-时常用到的-api)
        - [1.8.2. redux 数据流的走向](#182-redux-数据流的走向)
        - [1.8.3. redux 和 mobx 的区别](#183-redux-和-mobx-的区别)
        - [1.8.4. redux-saga 和 redux-thunk 对比](#184-redux-saga-和-redux-thunk-对比)
        - [1.8.5. redux-saga 常用的 api](#185-redux-saga-常用的-api)
        - [1.8.6. umi](#186-umi)
    - [1.9. 组件的复用](#19-组件的复用)
- [2. Vue](#2-vue)
- [3. React 和 Vue 异同点](#3-react-和-vue-异同点)
    - [3.1. 相同点](#31-相同点)
        - [3.1.1. hooks](#311-hooks)
    - [3.2. 不同点](#32-不同点)
        - [3.2.1. hooks](#321-hooks)
        - [3.2.2. 其他](#322-其他)

<!-- /TOC -->
# 1. React

React 是一个声明式，用于构建用户界面的 JS 库。使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。

- 声明式设计
- 使用 VDOM，减少 DOM 的交互
- JSX 语法
- 组件化 代码复用

## 1.1. 生命周期

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

## 1.2. 组件之间怎么通信

- 父级传给子级：this.props
- 子级传给父级：ref
- 跨组件传递：context，redux

## 1.3. react 中 key 的作用是什么

- 为了在 diff 算法执行时更快的找到对应的节点，提高 diff 速度
- key 变化的时候，节点会重新渲染
- map 的时候尽量不要用 index 做 key 值
  > 举例： 如果希望可以在 B 和 C 之间加一个 F
  > Diff 算法默认执行起来是这样的
  > ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/89713889899F496AB5A92E2A52B8A163/5375)
  > 即把 C 更新成 F，D 更新成 C，E 更新成 D，最后再插入 E，是不是很没有效率？<br />
  > 所以可是使用 key 来给每个节点做一个唯一标识，Diff 算法就可以正确的识别此节点，找到正确的位置区插入新的节点。<br /> > ![](https://note.youdao.com/yws/public/resource/9791688f8f13043d64eb2ded545dc193/xmlnote/8E6617DF1C4E47D480CF5B377EF2A863/5403)

## 1.4. diff 算法

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

## 1.5. function Component / class Component todo

## 1.6. context

- React.createContext
- `<MyContext.Provider value={/* some value */}>`
- `MyClass.contextType = MyContext;` 可以写一个 HOC，这个写在 HOC 中，就不需要每个用到的都写一遍
- MyContext.Provider
- MyContext.Consumer

## 1.7. hooks

### 1.7.1. 优点
- 更好的逻辑复用与代码组织
- 减小了代码体积
- 不用考虑 `this`
- 不编写 class 的情况下使用 state

### 1.7.2. 常用hooks
- useState
- useEffect
- useContext
- useReducer
- useMemo
- useRef
- 自定义 hook 与普通函数一样，只是:
  - 名称以 “use” 开头
  - 函数内部可以调用其他的 Hook


## 1.8. redux

- store: 将整个应用的 state 储存在唯一的 store 中。
- state 只能通过触发 action 来修改，其中 action 就是一个描述性的普通对象。
- reducer: 接收 action 和当前 state 作为参数，返回一个新的 State

### 1.8.1. 使用 redux 时，常用到的 api

- redux.createStore: `createStore(reducer, middleware)`
- redux.combineReducers
- store.getState
- store.dispatch
- [react-redux].connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])

> 参考：[Redux 入门教程，应用的状态管理器](https://www.jianshu.com/p/d296a8c34936)

### 1.8.2. redux 数据流的走向

![](https://raw.githubusercontent.com/dream-approaching/pictureMaps/master/img/20200915175323.png)

- 用户发出 Action: store.dispatch(action);
- Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action
- Reducer 会返回新的 State
- State 一旦有变化，Store 就会调用监听函数
- listener 可以通过 store.getState()得到当前状态，触发重新渲染 View

### 1.8.3. redux 和 mobx 的区别

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

### 1.8.4. redux-saga 和 redux-thunk 对比

- redux-saga
  - dispatch 一个简单对象，利用 generator 的思想处理异步
  - 优点: 异步控制清晰，并发处理方便
  - 缺点: 代码书写较多，上手较慢
- redux-thunk
  - 在 UI 组件中触发任务，dispatch 一个 action 生成函数，在这个函数中处理异步
  - 优点: 简单
  - 缺点: action 变得复杂，后期可维护性降低, 协调并发任务比较困难

### 1.8.5. redux-saga 常用的 api

- createSagaMiddleware: 创建一个 Redux middleware，并将 Sagas 连接到 Redux Store。
- takeEvery: 在发起（dispatch）到 Store 并且匹配 pattern 的每一个 action 上派生一个 saga。
- take: 创建一个 Effect 描述信息，用来命令 middleware 在 Store 上等待指定的 action
- put: 用于触发 action，功能上类似于 dispatch。
- call: 用于调用异步逻辑，支持 promise 。
- select: 可以取到 state 数据

### 1.8.6. umi

定位是开发框架，包含工具 + 路由

- 优点
  - 开箱即用: webpack + react router
  - 做了很多优化: Tree Shaking、公共文件的智能提取、按需加载

> **FAQ: umi 有啥特别的，工具用 webpack + webpack-dev-server + babel + postcss + ... ，路由用 react-router 不就完了吗?**  
> 这是上一代的使用方式，工具是工具，库是库，泾渭分明。而近来，我们发现工具和库其实可以糅合在一起，工具也是框架的一部分。 通过约定、自动生成和解析代码等方式来辅助开发，减少开发者要写的代码量。next.js 如此，umi 也如此

> 参考: [Hello！umi](https://zhuanlan.zhihu.com/p/33455048)

## 1.9. 组件的复用

- High order component
- render props
- custom hooks

# 2. Vue
# 3. React 和 Vue 异同点

## 3.1. 相同点
### 3.1.1. hooks

## 3.2. 不同点
### 3.2.1. hooks
- react hook底层是基于链表实现，调用的条件是每次组件被render的时候都会顺序执行所有的hooks；
- vue hook是基于用proxy实现的数据响应机制，只要任何一个更改data的地方，相关的function或者template都会被重新计算，因此避开了react可能遇到的性能上的问题。
### 3.2.2. 其他

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