当 QPS（每秒查询数）达到峰值时，如果不加控制，系统可能出现超时、崩溃、服务雪崩等问题。应对方式一般从以下几个层面入手：

## 限流（Rate Limiting）

- 固定窗口 / 滑动窗口 / 漏桶 / 令牌桶算法
    
- 实现方式：
    
    - [Nginx 限流模块](服务端/nginx/限流)（`limit_req_zone`）
    - 应用层中间件（如 Spring Cloud Gateway、Express 中间件等）
    - Redis + Lua 实现限流计数器

## 熔断与降级

当依赖服务不可用时，及时中断调用并返回降级响应，避免连锁反应：

- 熔断：如 Hystrix、Sentinel 的熔断机制
- 降级：提供默认值、缓存数据、静态页面等

## 缓存

减轻对后端服务的访问压力：

- 页面级缓存（如 [CDN](浏览器/HTTP/CDN)）
- 接口缓存（如 [Redis](服务端/redis/redis) 缓存热点数据）
- 本地缓存（如 Caffeine、Guava）

## 异步与排队

将部分请求异步化或进入排队队列以缓冲压力：

- 消息队列（如 Kafka、RabbitMQ）
- 任务排队（如异步下单、异步发送邮件）

## 系统扩容

如果系统架构允许，可进行水平扩展（加机器、加节点）：

- Auto Scaling（云服务中自动扩容）
- Kubernetes HPA（基于 CPU 或 QPS 的自动扩容）

## 热点隔离 & 分流

将高频操作进行独立处理或分流处理：

- 接口分组处理（VIP 用户和普通用户分开）
- 对热点 Key 做专门隔离（如 Redis 热点 Key 拆分）

## 监控与预警

提前发现 QPS 激增并及时处理：

- Grafana + Prometheus
- ELK + AlertManager