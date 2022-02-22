<!-- TOC -->

- [1. 浏览器](#1-浏览器)
    - [1.1. 浏览器缓存](#11-浏览器缓存)
    - [1.2. 浏览器的本地存储 session、cookie、localstorage、sessionStorage、IndexDB](#12-浏览器的本地存储-sessioncookielocalstoragesessionstorageindexdb)
    - [1.3. 输入一个网址到页面展示，发生了什么事情](#13-输入一个网址到页面展示发生了什么事情)
    - [1.4. TCP 三次握手 四次挥手](#14-tcp-三次握手-四次挥手)
    - [1.5. 常见状态码](#15-常见状态码)
    - [1.6. http 请求头里都有什么内容](#16-http-请求头里都有什么内容)
    - [1.7. http 和 https 的区别](#17-http-和-https-的区别)
    - [1.8. http 1.0、http 1.1 和 http 2.0 的区别](#18-http-10http-11-和-http-20-的区别)
    - [1.9. http 3.0](#19-http-30)
    - [1.10. 跨域以及常见解决办法](#110-跨域以及常见解决办法)
    - [1.11. XSS 攻击](#111-xss-攻击)
    - [1.12. CSRF 攻击](#112-csrf-攻击)

<!-- /TOC -->
# 1. 浏览器

## 1.1. 浏览器缓存

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

## 1.2. 浏览器的本地存储 session、cookie、localstorage、sessionStorage、IndexDB

- cookie：保存在浏览器端， 大小 4K，可以设置过期时间
- session：保存在服务器端，无大小限制
- localstorage：除非手动清除，否则永久保存，大小 5M
- sessionStorage：与 localStorage 类似，不过浏览器关闭时会自动清空
  ![](https://user-gold-cdn.xitu.io/2018/12/14/167ac245e669b3b3?imageslim)
- IndexDB: 支持的浏览器比较广泛，大小限制通常约为 50MB。

## 1.3. 输入一个网址到页面展示，发生了什么事情

- DNS 解析:将域名解析成 IP 地址
- TCP 连接：TCP 三次握手
- 发送 HTTP 请求
- 服务器处理请求(期间可能会读取服务器缓存或查询数据库)，并返回响应报文
- 浏览器根据返回的状态码判断是下载还是从缓存读取文件内容
- 浏览器解析渲染页面
- 断开连接：TCP 四次挥手
  > - 参考：[掘金——浏览器基础](https://juejin.im/post/5c137e7c6fb9a049f7461639#heading-2)
  > - 参考：[掘金——从 URL 输入到页面展现到底发生什么？](https://juejin.im/post/5bf3ad55f265da61682afc9b)

## 1.4. TCP 三次握手 四次挥手

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

## 1.5. 常见状态码

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

## 1.6. http 请求头里都有什么内容

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

## 1.7. http 和 https 的区别

- HTTPS 协议需要到 CA 申请证书，一般免费证书很少，需要交费
- HTTP 协议运行在 TCP 之上，所有传输的内容都是明文，HTTPS 运行在 SSL/TLS 之上，SSL/TLS 运行在 TCP 之上，所有传输的内容都经过加密的。
- HTTP 和 HTTPS 端口也不一样，前者是 80，后者是 443。
- HTTPS 可以有效的防止运营商劫持
- HTTPS 比 HTTP 更加安全，对搜索引擎更友好，利于 SEO

## 1.8. http 1.0、http 1.1 和 http 2.0 的区别

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

## 1.9. http 3.0

- 2018 年，QUIC 演变成为 HTTP3。
- 占比约 5%
- HTTP3 的主要改进在传输层上。不走 TCP 连接了，走 UDP。

> - 参考：[秒懂科普！HTTP 3 如此简单](https://segmentfault.com/a/1190000023929858)

## 1.10. 跨域以及常见解决办法

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

## 1.11. XSS 攻击

- 全称是 Cross Site Scripting(即跨站脚本) 为了和 CSS 区分，故叫它 XSS
- 攻击的实现有三种方式
  - 存储型：常见的场景是留言评论区提交一段脚本代码，如果前后端没有做好转义的工作，那评论内容存到了数据库，在页面渲染过程中直接执行, 相当于执行一段未知逻辑的 JS 代码
  - 反射型：如`http://sanyuan.com?q=<script>alert("你完蛋了")</script>`
  - 文档型：WIFI 路由器劫持或者本地恶意软件
- 防范措施：千万不要相信任何用户的输入！
  - 无论是在前端和服务端，都要对用户的输入进行转码或者过滤
  - 利用 CSP：即浏览器中的内容安全策略，它的核心思想就是服务器决定浏览器加载哪些资源
  - 利用 HttpOnly：很多 XSS 攻击脚本都是用来窃取 Cookie, 而设置 Cookie 的 HttpOnly 属性后，JavaScript 便无法读取 Cookie 的值。这样也能很好的防范 XSS 攻击。

## 1.12. CSRF 攻击

- CSRF(Cross-site request forgery), 即跨站请求伪造，指的是黑客诱导用户点击链接，打开黑客的网站，然后黑客利用用户目前的登录状态发起跨站请求。
- 防范措施

  - 利用 Cookie 的 SameSite 属性：SameSite 可以设置为三个值，Strict、Lax 和 None
  - 验证来源站点：需要要用到请求头中的两个字段: Origin 和 Referer。
  - 校验 token
