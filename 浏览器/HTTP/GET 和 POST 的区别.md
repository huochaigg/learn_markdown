
## 基本语义

|方法|含义|用途示例|
|---|---|---|
|GET|**获取资源**（查）|查询文章、获取用户信息等|
|POST|**提交资源/处理请求**（增）|提交表单、新建用户、上传数据|

## 主要区别详解

| 对比项                   | GET                         | POST                                |
| --------------------- | --------------------------- | ----------------------------------- |
| **请求参数位置**            | 放在 URL 中：`/api/user?id=123` | 放在请求体中（body）：`{id:123}`             |
| **可见性**               | 参数明文显示在地址栏，**不安全**          | 参数藏在请求体中，相对更安全                      |
| **浏览器缓存**             | 会被浏览器缓存                     | 不会被缓存                               |
| **书签收藏**              | 可以直接收藏                      | 不可以                                 |
| **幂等性**（重要）           | 是，刷新多次不会造成影响                | 否，刷新可能会导致数据重复创建等问题                  |
| **长度限制**              | URL 长度有限（一般 ≤ 2KB）          | 无明显限制，可上传大量数据（如文件）                  |
| **默认 `Content-Type`** | `text/plain` 或无             | `application/x-www-form-urlencoded` |
| **语义强调**              | 获取资源，不应修改服务端数据              | 提交数据，可能引起服务端状态变化                    |

- `GET` 请求就像你在浏览网页，点击链接或加载页面——**只读，不动服务器数据**
    
- `POST` 请求就像你提交表单、登录、发布微博——**会产生动作或副作用**



## POST请求比GET请求安全，为什么

**POST 请求相对 GET 来说“更安全一些”，是因为它**：

|特性|POST|GET|
|---|---|---|
|参数暴露|放在请求体中（body）|放在 URL 中，容易被记录、缓存、截获|
|浏览器缓存|默认不会缓存|可能被缓存，历史记录中可见|
|网络日志（如代理/网关）|请求体通常不会被记录|URL 常常被完整记录|
|长度限制|参数理论上无限长|URL 有长度限制（一般 2KB 左右）|

所以 POST 更不容易被“意外泄露”或“无意中触发”，也**更适合传输敏感数据（如密码、token）**。

更安全” ≠ “绝对安全

- POST 请求依然可以被拦截、篡改；
    
- 如果没有 HTTPS，加密体现在 TCP 层之上，POST/GET 都会被抓包；
    
- 安全性最终取决于：**是否使用 HTTPS**、**是否有身份认证机制**、**服务端是否验证签名和权限**。


## 真实安全性顺序（从高到低）是：

```
使用 HTTPS 的 POST > 使用 HTTPS 的 GET > HTTP 的 POST > HTTP 的 GET
```

| 场景                 | GET（不推荐）                      | POST（推荐）                          |
| ------------------ | ----------------------------- | --------------------------------- |
| 登录密码传输             | `/login?user=tom&pass=123456` | `{ user: 'tom', pass: '123456' }` |
| 请求被中间代理记录          | URL 被记录                       | body 一般不会记录                       |
| token 传递（非 header） | token 曝露在 URL（危险）             | token 在 body 中，相对更隐蔽              |
| 浏览器历史记录            | 参数可见且可复制                      | 不会在地址栏中出现                         |

所以：

- **不是 POST 比 GET 更安全！**  
    只是因为 GET 参数暴露在 URL 上，容易被缓存/记录，**不适合传密码或敏感信息**。
    
- 真正的安全要依靠 **HTTPS 加密 + 后端验证 + CSRF/XSS 防护等机制**。


## GET 请求缓存：浏览器是怎么缓存的

浏览器缓存 GET 请求，主要有两种机制：

| 缓存方式        | 控制字段（响应头）                      | 特点                        |
| ----------- | ------------------------------ | ------------------------- |
| 强缓存（强制使用缓存） | `Cache-Control`, `Expires`     | **不会向服务器发送请求，直接使用本地缓存**   |
| 协商缓存（条件请求）  | `ETag` / `Last-Modified` + 请求头 | 向服务器发请求，服务器决定是否返回新内容或 304 |

#### 强缓存示例：

```
Cache-Control: max-age=3600
```

- 表示浏览器可以缓存 3600 秒
- 在这段时间内再次请求相同 URL，浏览器会**直接使用本地副本**，不会请求服务器


#### 协商缓存示例：

服务端响应头：

```
ETag: "abc123"
```

下次请求时，浏览器发送：

```
If-None-Match: "abc123"
```

- 如果服务器判断内容没变，会返回 304 Not Modified，告诉浏览器用本地缓存
    
- 否则返回新内容（新的 200）

## GET 的幂等性是什么意思？

简单定义：

**多次执行对服务端的结果没有副作用**，不论执行一次还是多次，效果都一样。

|特性|GET 请求|
|---|---|
|是否可缓存|是，浏览器会按响应头机制缓存|
|幂等性|有，多次请求不会影响服务端状态|
|缓存好处|提高性能、节省带宽、减轻服务器压力|
使用POST会更改服务端，数据库的状态，比如创建订单，新增用户

但是如果用GET来创建订单和新增用户，就违反了设计原则
