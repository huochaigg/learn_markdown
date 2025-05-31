
# nodejs有哪些常用模块

## 内置模块

### 1.文件与路径操作

| 模块                          | 说明                |
| --------------------------- | ----------------- |
| [fs](服务端/nodejs/fs)         | 文件系统操作，如读写文件      |
| [path](服务端/nodejs/path)     | 路径处理，如拼接、解析路径     |
| [os](服务端/nodejs/os)         | 操作系统信息，如 CPU、内存   |
| [crypto](服务端/nodejs/crypto) | 加密解密，如哈希、HMAC、RSA |
| [stream](服务端/nodejs/stream) | 流操作，如大文件读写        |

### 2. 网络与服务

| 模块            | 说明                                     |
| ------------- | -------------------------------------- |
| `http`        | 创建 [HTTP](浏览器/HTTP/HTTP) 服务            |
| `https`       | 创建 HTTPS 服务                            |
| `net`         | [TCP](浏览器/HTTP/TCP协议)/Socket 通信        |
| `dns`         | DNS 查询                                 |
| `url`         | URL 解析与格式化                             |
| `querystring` | URL 查询字符串处理（老模块，推荐用 `URLSearchParams`） |

### 3. 工具与控制

| 模块              | 说明                          |
| --------------- | --------------------------- |
| `events`        | 事件机制（发布-订阅）                 |
| `child_process` | 启动子进程执行命令                   |
| `util`          | 工具函数，如 promisify            |
| `timers`        | 定时器（setTimeout、setInterval） |
| `assert`        | 断言工具（用于测试）                  |


## 常见第三方模块（需安装）

### 1. Web 开发

| 模块                               | 说明                         |
| -------------------------------- | -------------------------- |
| [express](服务端/express/express大纲) | Web 框架，处理路由、请求、响应          |
| `koa`                            | 更现代的 Web 框架，基于 async/await |
| `cors`                           | 处理跨域请求                     |
| `body-parser`                    | 解析请求体（Express4.16+ 已内置）    |
| `morgan`                         | HTTP 请求日志中间件               |


### 2. 数据库

| 模块                               | 说明                     |
| -------------------------------- | ---------------------- |
| [mongoose](服务端/mongodb/mongoose) | MongoDB ODM            |
| `mysql2`                         | 操作 MySQL               |
| `sequelize`                      | 支持多数据库的 ORM            |
| `redis`                          | Redis 客户端              |
| [prisma](服务端/prisma/prisma大纲)    | 现代化 ORM（TypeScript 友好） |

### 3. 工具类

| 模块                         | 说明             |
| -------------------------- | -------------- |
| `lodash`                   | 工具库，提供常用数据处理函数 |
| `moment`/`dayjs`           | 处理日期时间         |
| `axios`                    | HTTP 客户端       |
| `dotenv`                   | 加载 `.env` 环境变量 |
| `chalk`                    | 控制台输出彩色信息      |
| `yargs`                    | 命令行参数解析        |
| [commander](服务端/nodejs/命令) | 命令             |

### 4. 安全与加密

| 模块                                   | 说明        |
| ------------------------------------ | --------- |
| `jsonwebtoken`                       | JWT 生成与验证 |
| [bcrypt/bcryptjs](服务端/nodejs/bcrypt) | 密码加密      |

### 5. 测试与调试

| 模块                   | 说明                    |
| -------------------- | --------------------- |
| `mocha`              | 测试框架                  |
| [jest](测试/jest/jest) | 全能测试框架（也可用于前端）        |
| `supertest`          | HTTP 请求测试             |
| `nodemon`            | 自动重启 Node.js 应用（开发时用） |
