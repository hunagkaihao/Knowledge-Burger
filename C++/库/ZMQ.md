# **ZeroMQ（ZMQ）库详解**

**ZeroMQ**（ØMQ / 0MQ）是一个**高性能、无代理**（brokerless），而非传统意义上的消息中间件（如 RabbitMQ、Kafka）。它将复杂的网络通信抽象为“套接字”（socket），让开发者能像操作本地对象一样进行跨线程、跨进程、跨机器的通信。

------

## **🔑 核心特点**

| 特性                | 说明                                                         |
| :------------------ | :----------------------------------------------------------- |
| **无代理架构**      | 消息直接在端点间传输，无需中心服务器，降低延迟和单点故障风险 |
| **多种通信模式**    | 内置请求-响应、发布-订阅、推拉等模式，适配不同场景           |
| **跨平台 & 多语言** | 支持 Windows/Linux/macOS，提供 C/C++/Python/Java 等数十种语言绑定 |
| **高吞吐低延迟**    | 单机可达数百万消息/秒，微秒级延迟                            |
| **嵌入式设计**      | 库直接链接到你的程序中，无需额外部署服务                     |

------

## **🧩 核心概念**

### **1.** ***上下文（Context）***

- 每个进程通常只需一个 `zmq::context_t`
- 管理 I/O 线程和资源，是线程安全的
- 所有套接字都从上下文中创建

### **2.** ***套接字类型（Socket Types）***

ZeroMQ 定义了**语义化的套接字类型**，必须成对使用：

| 模式          | 发送端       | 接收端       | 典型用途                     |
| :------------ | :----------- | :----------- | :--------------------------- |
| **请求-响应** | `ZMQ_REQ`    | `ZMQ_REP`    | RPC、客户端-服务端交互       |
| **发布-订阅** | `ZMQ_PUB`    | `ZMQ_SUB`    | 广播通知、日志分发、行情推送 |
| **推-拉**     | `ZMQ_PUSH`   | `ZMQ_PULL`   | 并行任务分发、负载均衡       |
| **高级路由**  | `ZMQ_DEALER` | `ZMQ_ROUTER` | 自定义路由、异步代理         |

> ⚠️ 注意：不能混用类型（如 `PUB` 不能连接 `PULL`）

### **3.** ***传输协议***

通过地址指定通信方式：

- `inproc://name`：线程间通信（最快）
- `ipc:///tmp/file`：进程间通信（Unix 域套接字）
- `tcp://127.0.0.1:5555`：跨机器 TCP 通信
- `pgm://...`：多播（需支持）

------

## **💻 C++ 使用示例（基于 cppzmq）**

### **环境准备**

```c++
# Ubuntu/Debian
sudo apt install libzmq3-dev

# 代码中包含头文件（cppzmq 是纯头文件库）
#include <zmq.hpp>
```

### **示例 1：请求-响应模式（REQ/REP）**

**服务端**（rep_server.cpp）

```C++
#include <zmq.hpp>
#include <string>
#include <iostream>

int main() {
    zmq::context_t ctx;
    zmq::socket_t sock(ctx, ZMQ_REP);
    sock.bind("tcp://*:5555");

    while (true) {
        zmq::message_t request;
        sock.recv(request, zmq::recv_flags::none);
        std::cout << "Received: " << request.to_string() << std::endl;

        zmq::message_t reply("World", 5);
        sock.send(reply, zmq::send_flags::none);
    }
}
```

**客户端**（req_client.cpp）

```c++
#include <zmq.hpp>
#include <string>
#include <iostream>

int main() {
    zmq::context_t ctx;
    zmq::socket_t sock(ctx, ZMQ_REQ);
    sock.connect("tcp://localhost:5555");

    zmq::message_t request("Hello", 5);
    sock.send(request, zmq::send_flags::none);

    zmq::message_t reply;
    sock.recv(reply, zmq::recv_flags::none);
    std::cout << "Reply: " << reply.to_string() << std::endl;
}
```

### **示例 2：发布-订阅模式（PUB/SUB）**

**发布者**

```c++
zmq::context_t ctx;
zmq::socket_t publisher(ctx, ZMQ_PUB);
publisher.bind("tcp://*:5556");

while (true) {
    std::string topic = "weather";
    std::string data = "25°C";
    
    // 多段消息：主题 + 数据
    publisher.send(zmq::buffer(topic), zmq::send_flags::sndmore);
    publisher.send(zmq::buffer(data), zmq::send_flags::none);
    
    std::this_thread::sleep_for(1s);
}
```

**订阅者**

```c++
zmq::context_t ctx;
zmq::socket_t subscriber(ctx, ZMQ_SUB);
subscriber.connect("tcp://localhost:5556");
subscriber.set(zmq::sockopt::subscribe, "weather"); // 订阅主题

while (true) {
    zmq::message_t topic, data;
    subscriber.recv(topic, zmq::recv_flags::none);
    subscriber.recv(data, zmq::recv_flags::none);
    std::cout << topic.to_string() << ": " << data.to_string() << std::endl;
}
```

------

## **⚙️ 关键 API 说明**

| 操作       | C++ 方法                               |
| :--------- | :------------------------------------- |
| 创建上下文 | `zmq::context_t ctx;`                  |
| 创建套接字 | `zmq::socket_t sock(ctx, ZMQ_XXX);`    |
| 绑定地址   | `sock.bind("tcp://*:5555");`           |
| 连接地址   | `sock.connect("tcp://host:5555");`     |
| 发送消息   | `sock.send(zmq::buffer(data), flags);` |
| 接收消息   | `sock.recv(msg, flags);`               |
| 设置选项   | `sock.set(zmq::sockopt::linger, 0);`   |

> 💡 **重要提示**：
>
> - **先 `bind()` 再 `connect()`**（避免消息丢失）
> - PUB/SUB 模式中，**订阅者只接收连接后的消息**
> - 使用 `zmq::send_flags::sndmore` 发送多段消息

------

## **🛠️ 常见优化技巧**

1. **设置 linger 时间**（避免程序退出时阻塞）

   ```
   sock.set(zmq::sockopt::linger, 0); // 立即关闭
   ```

2. **增加 I/O 线程**（高并发场景）

   ```
   zmq::context_t ctx(4); // 4 个 I/O 线程
   ```

3. **使用非阻塞发送/接收**

   ```
   if (sock.send(msg, zmq::send_flags::dontwait)) {
       // 成功发送
   }
   ```

4. **消息压缩**（大数据量）

   ```
   sock.set(zmq::sockopt::conflate, true); // 只保留最新消息
   ```

------

## **🚫 常见误区**

| 误区                 | 正确做法                                                     |
| :------------------- | :----------------------------------------------------------- |
| 把 ZMQ 当作队列      | ZMQ 是**消息传递库**，不保证持久化（除非用磁盘队列扩展）     |
| 混淆 bind/connect    | **稳定端点用 bind**（如服务端），**动态端点用 connect**（如客户端） |
| 忽略消息边界         | ZMQ 保证**消息原子性**，但应用层需处理消息解析               |
| 在构造函数中启动线程 | 遵循“构造不启动”原则，单独提供 `start()` 方法                |

------

## **✅ 适用场景**

- **机器人控制系统**（如你代码中的调度系统）：模块间通信
- **金融行情分发**：低延迟广播
- **微服务内部通信**：替代 HTTP/RPC
- **日志收集系统**：PUSH/PULL 负载均衡
- **游戏服务器**：玩家状态同步

------

## **📚 总结**

> **ZeroMQ 的核心价值在于：用最简单的 API，实现最灵活的分布式通信。**
>
> - 不要把它当作“消息队列”，而是一个**智能套接字库**
> - 选择正确的**通信模式**比写代码更重要
> - 结合 `std::shared_ptr` 管理生命周期，避免资源泄漏
> - 在你的调度系统中，ZMQ 很可能用于 **WCS 与设备模块间的指令下发/状态上报**

建议阅读 [官方指南](https://zeromq.org/) 和《Code Connected》一书深入理解其哲学。