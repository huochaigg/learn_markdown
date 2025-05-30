HTTP/2 的多路复用（Multiplexing）是 HTTP/2 最核心、最显著的性能优化特性之一，目的是解决 HTTP/1.1 中**“队头阻塞（Head-of-Line Blocking）”**和**“一个连接只能处理一个请求”**的问题。

## HTTP/1.1 的痛点（对比前提）

- 每个请求必须单独建立一条 [TCP](浏览器/HTTP/TCP协议) 连接，或复用后按顺序响应。
- **一个请求未返回，后面的请求必须排队**（队头阻塞）。
- 你可能看到网站中会打开 6 个连接（Chrome 默认最大并发）来抢速度。
- 大量小文件（JS、CSS、图像）请求时浪费资源、效率低。


## HTTP/2 的多路复用核心点

### 一条连接，多个并发流

> 所有请求和响应都通过一个 TCP 连接进行复用，彼此不阻塞。

- 请求被拆分成多个“**帧（Frame）**”，通过一个连接发送。
- 每个请求分配一个独立的 **流 ID**，像通道一样区分。
- 请求和响应的数据帧可以**交错**发送，再根据流 ID 重组。

## 优势

| 优点    | 描述                 |
| ----- | ------------------ |
| 并发提升  | 多个请求可并行发送，不受顺序限制   |
| 无队头阻塞 | 某个请求慢不会影响其他请求      |
| 降低连接数 | 只需 1 个 TCP 连接，节省资源 |
| 减少延迟  | 减少 TCP 握手和慢启动问题    |

## 如何验证浏览器是否启用 HTTP/2 + 多路复用？

### 方法 1：Chrome 开发者工具

1. 打开 DevTools → `Network` 面板
2. 刷新页面，请求列表中右键 → 添加列 → 勾选 `Protocol`
3. 查看 `Protocol` 列是否为 `h2`（即 HTTP/2）

如果是 `h2`，说明该请求是通过 HTTP/2 协议发出的，具有多路复用能力。

### 命令行工具

```
curl -I --http2 https://example.com
```


## 如何开启 HTTP/2？

[Nginx](服务端/nginx/nginx) 配置（HTTPS 场景）

```
server {
    listen 443 ssl http2;
    server_name yourdomain.com;

    ssl_certificate     /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        root /var/www/html;
        index index.html;
    }
}
```

Nginx 要支持 HTTP/2，**必须启用 HTTPS（TLS）**。

[Node.js](服务端/nodejs/nodejs大纲)（示例）（[http和https](服务端/nodejs/http和https)）

```
const http2 = require('http2');
const fs = require('fs');

const server = http2.createSecureServer({
  cert: fs.readFileSync('cert.pem'),
  key: fs.readFileSync('key.pem'),
});

server.on('stream', (stream, headers) => {
  stream.respond({
    'content-type': 'text/html',
    ':status': 200,
  });
  stream.end('<h1>Hello HTTP/2</h1>');
});

server.listen(8443);
```