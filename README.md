# PythonStudy
Python学习过程中的一些分享

## HTTP协议

定义：HTTP 协议全称(HyperText Transfer Protocol)，翻译过来就是**超文本传输协议**。
作用：**规定了浏览器和 Web 服务器通信数据的格式**，也就是说**浏览器和web服务器通信需要使用http协议**。
HTTP使用的传输层协议为**TCP协议**，而网络层使用的是**IP协议**（当然还使用了很多其他协议），所以说**HTTP是一个基于TCP/IP协议簇来传递数据**。
 - **传输 HTTP 协议格式的数据是基于 TCP 传输协议的，发送数据之前需要先建立连接。**
默认端口号：80
特点：
 - **支持客户/服务器模式**
 - **简单快速：客户向服务器请求服务时，只需传送请求方法和路径**
 - **灵活：HTTP允许传输任意类型的数据对象**
   - 正在传输的类型由Content-Type（Content-Type是HTTP包中用来表示内容类型的标识）加以标记。
 - **无连接：限制每次连接只处理一个请求**
   - 服务器处理完客户的请求，并收到客户的应答后，即断开连接
   - 优点：节省传输时间
 - **无状态：协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态**。
   - 即我们给服务器发送 HTTP 请求之后，服务器根据请求，会给我们发送数据过来，但是，发送完，不会记录任何信息。

### HTTP常用请求头

1. Host(主机和端口号)
2. Connection(链接类型)
3. Upgrade-Insecure-Requests(升级为HTTPS请求)
4. ==User-Agent(浏览器名称)==
5. Accept-Encoding(传输文件类型)
6. Referer(页面跳转处)
7. Accept-Encoding(文件编解码格式)
8. ==Cookie(Cookie)==
9. x-requested-with：XMLHttpRequest(是Ajax异步请求)


### 常用状态码

- 1**：信息，服务器收到请求，需要请求者继续执行操作
- 2**：成功，操作被成功接受并处理
- 3**：重定向，需要进一步的操作已完成请求
- 4**：客户端错误，请求包含语法错误或无法完成请求
- 5**：服务器错误，服务器在处理请求的过程中发生了错误


### 长短连接

在服务器响应完毕后，一次会话结束，此时连接是否断开我们需要区分HTTP版本：

- 在HTTP/1.0版本的时候，客户端与服务器完成一个请求/响应之后，会将之前建立的**TCP连接断开**，下次请求的时候又要重新建立TCP连接，这也被称为**短连接**

- 在HTTP1.0发布仅半年后（1997年1月） ，HTTP/1.1版本发布并带来一个新的功能：

  在客户端与服务器完成一次请求/响应之后，允许**不断开TCP连接**，这意味着下次请求就直接使用这个TCP连接而不再需要重新握手建立新连接，这也被称为**长连接**

**注意：长连接是指一次TCP连接允许多次HTTP会话，HTTP永远都是一次请求/响应，会话结束，HTTP本身不存在长连接之说。**

浏览器在请求时请求头中都会携带一个参数：**Connection:keep-alive**，这表示浏览器要求与服务器建立长连接，而服务器也可以设置是否愿意建立长连接

长连接优缺点：

优点：当网站中有大量静态资源(图片、css、js等)就可以开启长连接，也就是会说几张图片可以通过一次TCP连接发送。

缺点：当客户端请求一次之后不再请求，而服务器却开着长连接，资源被占用，会造成严重的浪费资源。

### 断开连接过程

**在建立TCP连接时是三次握手，而断开TCP连接是四次挥手！**

> 面试题
> 面试官：为什么http建立连接需要三次握手，不是两次或四次？
> 答：**三次是最少的安全次数**，两次不安全，四次浪费资源。


### 服务端响应
### HTTPS
HTTP + SSL
默认端口号：443

注：HTTPS比HTTP更安全，但是性能更低。

**注：浏览器渲染出来的页面与爬虫请求的页面并不一样**




## RESTful架构
**REST定义**：REST，即 `Representational State Transfer` 的缩写，我对这个词组的理解是"**资源表现层状态转化**"。如果一个架构符合REST原则，就称它为RESTful架构。
### RESTful架构：
* 每一个URL代表一种资源；
* 客户端和服务器之间，传递这种资源的某种表现层；
* 客户端通过四个HTTP动词(GET、POST、PUT、DELETE)，对服务器端资源进行操作，实现"表现层状态转化"。
### RESTful架构设计误区
* **URL中应该不能包含有动词**。因为"资源"表示一种实体，所以应该是名词，URL不应该有动词，**动词应该放在HTTP协议中**。
* **版本号应该放在HTTP请求头信息的Accept字段中进行区分**而不是URL中，因为不同的版本，可以理解成同一种资源的不同表现形式，所以应该采用同一个URI。
### Restful API设计指南
##### 一、协议：
* API与用户的通信协议，总是使用HTTPS协议(SSL/TLS)。
##### 二、域名：
* 应该尽量将API部署在专用域名之下；
* 如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。
##### 三、版本（Versioning）
* 应该将API的版本号放入URL。
##### 四、路径（Endpoint）
* 路径又称"终点"（endpoint），表示API的具体网址。在RESTful架构中，每个网址代表一种资源（resource），
* **注意**：
  * 网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。
  * API中的名词应该使用复数。无论子资源或者所有资源。
 ##### 五、HTTP动词
* 对于资源的具体操作类型，由HTTP动词表示。
* 常用的HTTP动词（括号里是对应的SQL命令）：
  * GET（SELECT）：从服务器取出资源（一项或多项）；
  * POST（CREATE）：在服务器新建一个资源；
  * PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）；
  * PATCH（UPDATE）：在服务器更新[部分]资源（客户端提供改变的属性）；
  * DELETE（DELETE）：从服务器删除资源。
* 不常用的HTTP动词：
  * HEAD：获取资源的元数据；
  * OPTIONS：获取信息，关于资源的哪些属性是客户端可以改变的。
##### 六、过滤信息（Filtering）
* 如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。
* 常见的参数：
  ```
  ?limit=10：指定返回记录的数量
  ?offset=10：指定返回记录的开始位置。
  ?page=2&per_page=100：指定第几页，以及每页的记录数。
  ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
  ?animal_type_id=1：指定筛选条件
  参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。
  ```
##### 七、状态码（Status Codes）
* 服务器向用户返回的状态码和提示信息，常见的有以下一些（方括号中是该状态码对应的HTTP动词）
  ```
  200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
  201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
  202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
  204 NO CONTENT - [DELETE]：用户删除数据成功。
  400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
  401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
  403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
  404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
  406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
  410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
  422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
  500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
  ```
##### 八、错误处理（Error handling）
* 如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将 error 作为键名，出错信息作为键值即可。
##### 九、返回结果
* 针对不同操作，服务器向用户返回的结果应该符合以下规范。
 * GET /collection：返回资源对象的列表（数组）
 * GET /collection/resource：返回单个资源对象
 * POST /collection：返回新生成的资源对象
 * PUT /collection/resource：返回完整的资源对象
 * PATCH /collection/resource：返回完整的资源对象
 * DELETE /collection/resource：返回一个空文档
##### 十、Hypermedia API
* RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么。
##### 十一、其他
1. API的身份认证应该使用OAuth 2.0框架。
2. 服务器返回的数据格式，应该尽量使用JSON，避免使用XML。
