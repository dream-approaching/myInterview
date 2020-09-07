## 目录

<!-- TOC -->

- [目录](#目录)
- [面试题](#面试题)
  - [如何解决a标签点击后hover事件失效的问题?](#如何解决a标签点击后hover事件失效的问题)

<!-- /TOC -->

## 面试题
### 如何解决a标签点击后hover事件失效的问题?  
  改变a标签css属性的排列顺序,只需要记住`LoVe HAte`原则就可以了：
  ```
  link→visited→hover→active
  ```
  比如下面错误的代码顺序：  
  ```css
  a:hover{
    color: green;
    text-decoration: none;
  }
  a:visited{ /* visited在hover后面，这样的话hover事件就失效了 */
    color: red;
    text-decoration: none;
  }
  ```