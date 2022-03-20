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
* 自 2005 年以来，可用于 Web 页面的 API 大大增加，其中几个 API 为特定目的扩展了 HTTP 协议，大部分是新的特定 HTTP 头： 
  * Server-sent events，服务器可以偶尔推送消息到浏览器。 
  * WebSocket，一个新协议，可以通过升级现有 HTTP 协议来建立。
* 放松Web的安全模型 
  * HTTP和Web安全模型--同源策略是互不相关的。事实上，当前的Web安全模型是在HTTP被创造出来后才被发展的！这些年来，已经证实了它如果能通过在特定的约束下移除一些这个策略的限制来管的宽松些的话，将会更有用。这些策略导致大量的成本和时间被花费在通过转交到服务端来添加一些新的HTTP头来发送。这些被定义在了Cross-Origin Resource Sharing (CORS) or the Content Security Policy (CSP)规范里。 
  * 不只是这大量的扩展，很多的其他的头也被加了进来，有些只是实验性的。比较著名的有Do Not Track (DNT) 来控制隐私，X-Frame-Options, 还有很多。
* HTTP/2在HTTP/1.1有几处基本的不同:
  * HTTP/2是二进制协议而不是文本协议。不再可读，也不可无障碍的手动创建，改善的优化技术现在可被实施。
  * 这是一个复用协议。并行的请求能在同一个链接中处理，移除了HTTP/1.x中顺序和阻塞的约束。
  * 压缩了headers。因为headers在一系列请求中常常是相似的，其移除了重复和传输重复数据的成本。
  * 其允许服务器在客户端缓存中填充数据，通过一个叫服务器推送的机制来提前请求。

### [HTTP消息](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Messages)

* HTTP 请求和响应具有相似的结构，由以下部分组成︰
  1. 一行起始行用于描述要执行的请求，或者是对应的状态，成功或失败。这个起始行总是单行的。
  2. 一个可选的HTTP头集合指明请求或描述消息正文。
  3. 一个空行指示所有关于请求的元数据已经发送完毕。
  4. 一个可选的包含请求相关数据的正文 (比如HTML表单内容), 或者响应相关的文档。 正文的大小有起始行的HTTP头来指定。
* HTTP/1.x 报文有一些性能上的缺点：
  * Header 不像 body，它不会被压缩。
  * 两个报文之间的 header 通常非常相似，但它们仍然在连接中重复传输。
  * 无法复用。当在同一个服务器打开几个连接时：TCP 热连接比冷连接更加有效。
* HTTP/2 引入了一个额外的步骤：它将 HTTP/1.x 消息分成帧并嵌入到流 (stream) 中。数据帧和报头帧分离，这将允许报头压缩。将多个流组合，这是一个被称为 多路复用 (multiplexing) 的过程，它允许更有效的底层 TCP 连接。HTTP 帧现在对 Web 开发人员是透明的。在 HTTP/2 中，这是一个在  HTTP/1.1 和底层传输协议之间附加的步骤。Web 开发人员不需要在其使用的 API 中做任何更改来利用 HTTP 帧；当浏览器和服务器都可用时，HTTP/2 将被打开并使用。

### [典型的 HTTP 会话](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Session)

* 使用 TCP 时，HTTP 服务器的默认端口号是 80，另外还有 8000 和 8080 也很常用。页面的 URL 会包含域名和端口号，但当端口号为 80 时可以省略。前往 [标识互联网上的内容](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web) 获取更多内容。
* 注意: 客户端-服务器模型不允许服务器在没有显式请求时发送数据给客户端。为了解决这个问题，Web 开发者们使用了许多技术：例如，使用 XMLHTTPRequest 或 Fetch API 周期性地请求服务器，使用 HTML WebSockets API，或其他类似协议。
* 注意最后的空行，它把首部与数据块分隔开。由于在 HTTP 首部中没有 Content-Length，数据块是空的，所以服务器可以在收到代表首部结束的空行后就开始处理请求。
* [HTTP 响应状态码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status) 用来表示一个 HTTP 请求是否成功完成。响应被分为 5 种类型：信息型响应，成功响应，重定向，客户端错误和服务端错误。
  * [200](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200): OK. 请求成功。
  * [301](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/301): Moved Permanently. 请求资源的 URI 已被改变。
  * [404](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/404): Not Found. 服务器无法找到请求的资源。
  * [503](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/503): Service Unavailable. 503 Service Unavailable 是一种HTTP协议的服务器端错误状态代码，它表示服务器尚未处于可以接受请求的状态。通常造成这种情况的原因是由于服务器停机维护或者已超载。注意在发送该响应的时候，应该同时发送一个对用户友好的页面来解释问题发生的原因。该种响应应该用于临时状况下，与之同时，在可行的情况下，应该在 Retry-After 首部字段中包含服务恢复的预期时间。 缓存相关的首部在与该响应一同发送时应该小心使用，因为 503 状态码通常应用于临时状况下，而此类响应一般不应该进行缓存。

### [HTTP/1.x 的连接管理](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Connection_management_in_HTTP_1.x)

* 有两个新的模型在 HTTP/1.1 诞生了。首先是长连接模型，它会保持连接去完成多次连续的请求，减少了不断重新打开连接的时间。然后是 HTTP 流水线模型，它还要更先进一些，多个连续的请求甚至都不用等待立即返回就可以被发送，这样就减少了耗费在网络延迟上的时间。
* HTTP 流水线在现代浏览器中并不是默认被启用的：
  * Web 开发者并不能轻易的遇见和判断那些搞怪的代理服务器的各种莫名其妙的行为。
  * 正确的实现流水线是复杂的：传输中的资源大小，多少有效的 RTT 会被用到，还有有效带宽，流水线带来的改善有多大的影响范围。不知道这些的话，重要的消息可能被延迟到不重要的消息后面。这个重要性的概念甚至会演变为影响到页面布局！因此 HTTP 流水线在大多数情况下带来的改善并不明显。
  * 流水线受制于 HOL 问题。
  * 由于这些原因，流水线已经被更好的算法给代替，如 multiplexing，已经用在 HTTP/2。
* 并不是所有类型的 HTTP 请求都能用到流水线：只有 idempotent 方式，比如 GET、HEAD、PUT 和 DELETE 能够被安全的重试：如果有故障发生时，流水线的内容要能被轻易的重试。
* 今天，所有遵循 HTTP/1.1 的代理和服务器都应该支持流水线，虽然实际情况中还是有很多限制：一个很重要的原因是，目前没有现代浏览器默认启用这个特性。
* 除非你有紧急而迫切的需求，不要使用这一过时的技术，升级到 HTTP/2 就好了。在 HTTP/2 里，做域名分片就没必要了：HTTP/2 的连接可以很好的处理并发的无优先级的请求。域名分片甚至会影响性能。大多数 HTTP/2 的实现还会使用一种称作连接凝聚的技术去尝试合并被分片的域名。
* [协议升级机制](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Protocol_upgrade_mechanism)
  * 协议升级请求总是由客户端发起的；暂时没有服务端请求协议更改的机制。当客户端试图升级到一个新的协议时，可以先发送一个普通的请求（GET，POST等），不过这个请求需要进行特殊配置以包含升级请求。
  * 如果服务器决定升级这次连接，就会返回一个 101 Switching Protocols 响应状态码，和一个要切换到的协议的头部字段Upgrade。 如果服务器没有（或者不能）升级这次连接，它会忽略客户端发送的 "Upgrade 头部字段，返回一个常规的响应：例如一个200 OK).

### [HTTP 请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

### [HTTP 响应代码](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status)

### [Firefox 开发者工具](https://developer.mozilla.org/zh-CN/docs/Tools)

### [Mozilla Observatory](https://observatory.mozilla.org/)

一个旨在帮助开发人员，系统管理员和安全专业人员安全地配置其站点的项目。

### [浏览器的工作原理](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)

一篇非常全面的关于浏览器内部实现与通过 HTTP 协议的请求流的文章。可以说是所有 Web 开发者的**必读内容**。


















































































