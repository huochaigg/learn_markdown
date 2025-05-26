Redis（Remote Dictionary Server）是一个开源的高性能键值（key-value）存储数据库，支持多种数据结构，如字符串、哈希、列表、集合、有序集合等。以下是Redis的基本使用和操作详解：

# 1. 安装 Redis
**linux**
```
# 安装 Redis
sudo apt update
sudo apt install redis-server -y

# 启动 Redis
sudo systemctl start redis
sudo systemctl enable redis

sudo systemctl disable redis // 停止

```
**mac（使用homebrew）**
```
brew install redis
brew services start redis

brew services stop redis // 停止开机自启
```
**Windows**
Windows 官方不再提供 Redis 版本，可以使用第三方编译版：https://github.com/tporadowski/redis
# 2. 启动数据库
```
redis-server

// 后台启动
redis-server --daemonize yes
```
# 3. 连接数据库
```
redis-cli

// 如果 Redis 运行在远程服务器：
redis-cli -h 127.0.0.1 -p 6379
```

Redis CLI 命令 切换数据库
```
SELECT 1  # 切换到数据库 1
```
# 4. 检查数据库进程
linux/macOs
```
ps aux | grep redis
```
windows
```
tasklist | findstr redis
```

# 5. Redis 基本数据类型操作

## 5.1 字符串（String）
写入数据
```
SET key1 "hello"
```
![](media/17425265858770.jpg)

读取数据
```
GET key1
```
删除数据
```
DEL key1
```
自增/自减
```
INCR counter   # counter +1
DECR counter   # counter -1
```
追加字符串
```
APPEND key1 " world"
```
## 5.2 哈希（Hash）
写入数据
```
HSET user:1 name "Tom" age 25
```
![](media/17425267928677.jpg)

读取数据
```
HGET user:1 name
HGETALL user:1
```
删除字段
```
HDEL user:1 age
```
## 5.3 列表（List）
![](media/17425298209419.jpg)

![](media/17425294922872.jpg)

从左侧/右侧插入
```
LPUSH mylist "apple"
RPUSH mylist "banana"
```
从左侧/右侧弹出
```
LPOP mylist
RPOP mylist
```
获取列表元素
```
LRANGE mylist 0 -1  # 获取所有元素
```
## 5.4 集合（Set）
添加元素
```
SADD myset "a" "b" "c"
```
![](media/17425266413382.jpg)

获取所有元素
```
SMEMBERS myset
```
检查元素是否存在
```
SISMEMBER myset "a"
```
取两个集合的交集
```
SINTER set1 set2
```
## 5.5 有序集合（Sorted Set）
添加元素
```
ZADD scores 100 "Alice" 200 "Bob"
```
![](media/17425267090100.jpg)

获取有序集合
```
ZRANGE scores 0 -1 WITHSCORES
```
获取排名
```
ZRANK scores "Alice"
```
# 6. 高级操作
## 6.1 过期时间
```
SETEX key 60 "temporary_data"  # 60秒后自动删除
EXPIRE key 30  # 给已有key设置30秒的TTL
```
## 6.2 事务
```
MULTI
SET key1 "val1"
SET key2 "val2"
EXEC
```
## 6.3 发布/订阅（Pub/Sub）
订阅频道
```
SUBSCRIBE mychannel
```
发布消息
```
PUBLISH mychannel "Hello Redis"
```
# 7. Redis 持久化

## 7.1 RDB（快照持久化）
```
SAVE  # 手动保存
BGSAVE  # 后台保存
```
## 7.2 AOF（追加日志持久化）
```
CONFIG SET appendonly yes
```
# 8. Redis 主从复制

设置主从
在从服务器上：
```
SLAVEOF <master-ip> <port>
```
这就是 Redis 的基本操作，你可以根据实际需求选择合适的数据结构和操作方式。想深入了解某些功能吗？ 😊