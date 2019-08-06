# PythonStudy
Python学习过程中的一些分享


# RESTful架构
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
对于资源的具体操作类型，由HTTP动词表示。
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
