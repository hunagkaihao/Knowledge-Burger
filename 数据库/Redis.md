# Redis简介

Redis是一种开源的内存数据结构存储系统，它支持多种数据结构，如字符串、哈希、列表、集合、有序集合等。Redis可以用作数据库、缓存和消息中间件，并在性能、可扩展性和灵活性方面表现出色。

Redis的核心特性

**高性能**: Redis提供极高的性能，每秒可以进行数十万次的读写操作。这使得Redis成为处理高并发请求的理想选择，尤其是在需要快速响应的场景中，如缓存、会话管理、排行榜等。

**丰富的数据类型**: Redis不仅支持基本的键值存储，还提供了丰富的数据类型，包括字符串、列表、集合、哈希表、有序集合等。这些数据类型为开发者提供了灵活的数据操作能力，使得Redis可以适应各种不同的应用场景。

**原子性操作**: Redis的所有操作都是原子性的，这意味着操作要么完全执行，要么完全不执行。这种特性对于确保数据的一致性和完整性至关重要，尤其是在高并发环境下处理事务时。

**持久化**: Redis支持数据的持久化，可以将内存中的数据保存到磁盘中，以便在系统重启后恢复数据。这为Redis提供了数据安全性，确保数据不会因为系统故障而丢失。

**支持发布/订阅模式**: Redis内置了发布/订阅模式（Pub/Sub），允许客户端之间通过消息传递进行通信。这使得Redis可以作为消息队列和实时数据传输的平台。

**单线程模型**: 尽管Redis是单线程的，但它通过高效的事件驱动模型来处理并发请求，确保了高性能和低延迟。单线程模型也简化了并发控制的复杂性。

**主从复制**: Redis支持主从复制，可以通过从节点来备份数据或分担读请求，提高数据的可用性和系统的伸缩性。

Redis的应用场景

Redis被广泛应用于各种场景，包括但不限于缓存系统、会话存储、排行榜、实时分析、地理空间数据索引等。它的高性能和灵活的数据结构使得Redis可以满足多种业务需求。

Redis与其他数据库的比较

与其他键值存储系统相比，Redis提供了丰富的数据类型和高性能的读写能力。它的所有操作都是原子性的，支持数据的持久化，并且拥有丰富的特性集，如发布/订阅模式、通知、键过期等。Redis还支持主从复制和高可用性，提供了数据的备份和主从复制功能，增强了数据的可用性和容错性。

总结

Redis是一个功能强大且灵活的数据库，可以根据不同的需求来使用。它可以用于内存存储、持久化、发布订阅系统、地图信息分析以及计时器和计数器等多种应用场景。Redis的高性能和丰富的数据结构使其成为当前最受欢迎的NoSQL数据库之一。

# Redis 安装

Redis 的安装访问地址：`https://github.com/ServiceStack/redis-windows/tree/master/downloads`把 Redis 下载下来后找到一个合适的地方解压，就能得到如下图所示的目录

![](D:\Project\Knowledge-Burger\Picture\C#\Redis\redis安装目录.png)

###### 便捷方式

为了方便启动，我们在该目录下新建一个 startup.cmd 的文件，然后将以下内容写入文件：`redis-server redis.windows.conf1`这个命令其实就是在调用 redis-server.exe 命令来读取 redis.window.conf 的内容，我们双击刚才创建好的 startup.cmd 文件，就能成功的看到 Redis 启动：

![](D:\Project\Knowledge-Burger\Picture\C#\Redis\运行redis.png)

上图的提示信息告诉了我们：① Redis 当前的版本为 3.0.503；② Redis 运行在 6379 端口；③ Redis 进程的 PID 为 14748；④ 64 位。我们可以打开同一个文件夹下的 redis-cli.exe 文件

这是 Redis 自带的一个客户端工具，它可以用来连接到我们当前的 Redis 服务器，我们做以下测试：如此，我们便在 Windows 的环境下安装好了 Redis

![](D:\Project\Knowledge-Burger\Picture\C#\Redis\redis-cli.png)

###### 安装后的测试

1.双击启动redis-cil 输入命令测试redis 的安装  `set key的名称 value` 会返回你ok 2.输入`get key的名称`就会返回你输入的信息，就证明你安装的redis没有问题

# Redis C# 中的使用

1、ServiceStack.Redis，据说是Redis官方推荐使用的驱动类库，但是是收费的。

2、StackExchange.Redis，可能性能要比ServiceStack.Redis差点，但是是免费的。（案例）

#### 安装`StackExchange.Redis` 通过netget或者命令安装

![](D:\Project\Knowledge-Burger\Picture\C#\Redis\nuget.png)

**创建连接**
使用`ConnectionMultiplexer`管理Redis连接：

```csharp
using StackExchange.Redis;
var redis = ConnectionMultiplexer.Connect("localhost:6379");
IDatabase db = redis.GetDatabase();
```

可以配置多个节点、密码或SSL选项以支持集群和安全连接。

**数据库编号**

Redis 默认内置了 16 个独立的数据库，编号从 `0` 到 `15`。默认情况下，客户端连接的是 `0` 号数据库。
你可以把 Redis 想象成一栋有 16 个独立房间的大楼，`mDbNum` 就是你要进入的“房间号”。不同房间里的数据是完全隔离的，比如在 0 号库里存了一个键 `user`，在 1 号库里也可以存一个键 `user`，它们互不影响。

```C#
IDatabase db = mRedisClient.GetDatabase(1);
```

## 基本数据操作

- **字符串操作**

```csharp
db.StringSet("key1", "Hello Redis");
string value = db.StringGet("key1");
Console.WriteLine(value);
```

- **哈希操作**

```csharp
db.HashSet("user:1001", new HashEntry[] {
    new HashEntry("name", "张三"),
    new HashEntry("age", 30)
});
var name = db.HashGet("user:1001", "name");
```

- **列表操作**

```csharp
db.ListLeftPush("tasks", "task1");
var task = db.ListRightPop("tasks");
```

- **集合与有序集合**

```csharp
db.SetAdd("tags", "C#");
db.SortedSetAdd("scores", "player1", 100);
```

- **键过期**

```csharp
db.StringSet("tempKey", "value", TimeSpan.FromMinutes(5));
```

#  Another-Redis-Desktop-Manager 使用

1. 新建连接

   点击 新建连接 创建新的连接

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\新建连接.png)

2. 填写连接信息

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\填写连接信息.png)

一般只需要填一下几个内容：

地址：redis地址 (哨兵地址)
哨兵端口：redis默认 6379  (哨兵默认 26379)
Redis Node Password：设置的Redis密码
Master Group Name: master 别名
连接名称：所新建连接的名称，不填会根据地址和端口自动生成

3. 设置

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\设置.png)

​	基础设置

​	![](D:\Project\Knowledge-Burger\Picture\C#\Redis\基础设置.png)

4. redis 的基本信息

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\redis的基本信息.png)

5. 新增数据

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\新增数据.png)

6. 填写key和数据类型

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\填写key和数据类型.png)

7. 填写value内容

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\填写value内容.png)

8. 多连接的颜色标记

   ![](D:\Project\Knowledge-Burger\Picture\C#\Redis\多连接颜色标记1.png)

   ![多连接颜色标记2](D:\Project\Knowledge-Burger\Picture\C#\Redis\多连接颜色标记2.png)

   ![多连接颜色标记3](D:\Project\Knowledge-Burger\Picture\C#\Redis\多连接颜色标记3.png)

   ![多连接颜色标记4](D:\Project\Knowledge-Burger\Picture\C#\Redis\多连接颜色标记4.png)