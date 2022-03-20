# [HTTP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

### [HTTP概述](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview)

* 代理（Proxies）: 在浏览器和服务器之间，有许多计算机和其他设备转发了HTTP消息。由于Web栈层次结构的原因，它们大多都出现在传输层、网络层和物理层上，对于HTTP应用层而言就是透明的，虽然它们可能会对应用层性能有重要影响。还有一部分是表现在应用层上的，被称为代理（Proxies）。代理（Proxies）既可以表现得透明，又可以不透明（“改变请求”会通过它们）。代理主要有如下几种作用：
  * 缓存（可以是公开的也可以是私有的，像浏览器的缓存）
  * 过滤（像反病毒扫描，家长控制...）
  * 负载均衡（让多个服务器服务不同的请求）
  * 认证（对不同资源进行权限管理）
  * 日志记录（允许存储历史信息）
* HTTP 是无状态，有会话的: HTTP是无状态的：在同一个连接中，两个执行成功的请求之间是没有关系的。这就带来了一个问题，用户没有办法在同一个网站中进行连续的交互，比如在一个电商网站里，用户把某个商品加入到购物车，切换一个页面后再次添加了商品，这两次添加商品的请求之间没有关联，浏览器无法知道用户最终选择了哪些商品。而使用HTTP的头部扩展，HTTP Cookies就可以解决这个问题。把Cookies添加到头部中，创建一个会话让每次请求都能共享相同的上下文信息，达成相同的状态。 **注意，HTTP本质是无状态的，使用Cookies可以创建有状态的会话。**

### [HTTP 缓存](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching)

### [HTTP cookies](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)

### [跨源资源共享（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)

### [HTTP的发展](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)

* HTTP 用于安全传输: 
  * HTTP最大的变化发生在1994年底。HTTP在基本的TCP/IP协议栈上发送信息，网景公司（Netscape Communication）在此基础上创建了一个额外的加密传输层：SSL 。SSL 1.0没有在公司以外发布过，但SSL 2.0及其后继者SSL 3.0和SSL 3.1允许通过加密来保证服务器和客户端之间交换消息的真实性，来创建电子商务网站。SSL在标准化道路上最终成为TLS,随着版本1.0, 1.1, 1.2的出现成功地关闭漏洞。TLS 1.3 目前正在形成。 
  * 与此同时，人们对一个加密传输层的需求也愈发高涨：因为 Web 最早几乎是一个学术网络，相对信任度很高，但如今不得不面对一个险恶的丛林：广告客户、随机的个人或者犯罪分子争相劫取个人信息，将信息占为己有，甚至改动将要被传输的数据。随着通过HTTP构建的应用程序变得越来越强大，可以访问越来越多的私人信息，如地址簿，电子邮件或用户的地理位置，即使在电子商务使用之外，对TLS的需求也变得普遍。



























































































