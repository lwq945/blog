## cookie 的工作原理
当服务器收到HTTP请求时，服务器可以在响应头里面添加一个Set-Cookie选项。浏览器收到响应后通常会保存下Cookie，之后对该服务器每一次请求中都通过Cookie请求头部将Cookie信息发送给服务器。另外，Cookie的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。
**服务器使用Set-Cookie响应头部向用户代理（一般是浏览器）发送Cookie信息。一个简单的Cookie可能像这样：**
```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[页面内容]
```
**现在，对该服务器发起的每一次新请求，浏览器都会将之前保存的Cookie信息通过Cookie请求头部再发送给服务器。**
```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```
**会话期Cookie**是最简单的Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。会话期Cookie不需要指定过期时间（Expires）或者有效期（Max-Age）。
**持久性Cookie**可以指定一个特定的过期时间（Expires）或有效期（Max-Age）。
```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```


**cookie的弊端**

1. cookie 中的所有数据在客户端可以被修改。这就意味着数据非常容易被伪造，一些重要的数据就不能存放在 cookie 中
2. cookie 中数据字段太多会影响传输效率。

## session的工作原理
1. 每个 session 都对应一个 session_id，通过 session_id 可以查询到对应的 session
2. session_id 通常是存放在客户端的 cookie 中，服务端存好 session 之后将对应的 session_id 设置在 cookie 中发送给客户端
3. 当请求到来时，服务端检查 cookie 中保存的 session_id 并通过这个 session_id 与服务器端的 session 关联起来，进行数据的保存和修改

当你浏览一个网页时，服务端随机产生一个很长的字符串，然后存在你 cookie 中。当你下次访问时，cookie 会带有这个字符串，然后浏览器就知道你是上次访问过的谁，然后从服务器的存储中取出上次记录在你身上的数据。由于字符串是随机产生的，而且位数足够多，所以也不担心有人能够伪造。

**session 的储存有四个常用选项：内存、 cookie、缓存、数据库**

## cookie和session经常结合使用
具体来说cookie机制采用的是在客户端保持状态的方案，而session机制采用的是在服务器端保持状态的方案。两者存储的都是用户登录信息，操作行为等等的数据。
- cookie是把用户的数据写在用户本地浏览器上, 其他网站也可以扫描使用你的cookie，容易泄露自己网站用户的隐私，而且一般浏览器对单个网站站点有cookie数量与大小的限制。
- Session是把用户的数据写在用户的独占session上，存储在服务器上，一般只将session的id存储在cookie中。但将数据存储在服务器对服务器的成本会高。session是由服务器创建的，开发人员可以在服务器上通过request对象的getsession方法得到session

当一个用户打开淘宝登录后，刷新浏览器仍然展示登录状态。服务器如何分辨这次发起请求的用户是刚才登录过的用户呢？由于**HTTP协议是无状态的**，所以并不知道是哪个用户操作的。这里就使用了session保存状态。

1. 用户在输入用户名、密码提交给服务端，服务端验证通过后会给这个用户创建一个session对象，用于记录用户的相关信息，这个 session 可保存在服务器内存中（容易产生内存泄露），生产环境一般是保存在数据库中。
2. 创建session后，会把关联的session_id 通过setCookie 添加到http响应头部中返回给浏览器。
3. 浏览器在加载页面时发现响应头部有 set-cookie字段，就把这个cookie 种到浏览器指定域名下。(保存到指定域名下)
4. 当下次刷新页面时，发送的请求会带上这条cookie， 服务端在接收到后根据这个session_id来识别用户。

## cookie和session的区别
1. session 保存在服务器端 (默认被存在在服务器的一个文件里,session 可以放在 文件、数据库、或内存中都可以。），cookie 保存在客户端（浏览器）
2. session 的运行依赖 session_id，而 session_id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id）
3. 用户验证这种场合一般会用 session
4. 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
5. cookie不是很安全(XSS攻击)，别人可以分析存放在本地的 cookie并进行cookie欺骗。cookie 中的所有数据在客户端可以被修改。这就意味着数据非常容易被伪造。
6. cookie的生命周期通过设置一个过期时间；没有设置的话，为浏览器会话期间，关闭访问服务器的浏览器窗口，cookie就消失了。服务器会把长时间没有活动的Session从服务器内存中清除，此时Session便失效。具体根据服务器设置，一般在二三十分钟左右。
