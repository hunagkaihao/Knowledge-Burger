# MQTT 简介

MQTT（Message queuing telemetry transport），是一种基于发布/订阅（publish/subscribe）模式的"轻量级"通讯协议，该协议构建于TCP/IP（也有UDP）协议上，由IBM在1999年发布。MQTT最大优点在于，可以以极少的代码和有限的带宽，为连接远程设备提供实时可靠的消息服务。作为一种低开销、低带宽占用的即时通讯协议，使其在物联网、小型设备、移动应用等方面有较广泛的应用。

# MQTT 特点

![](D:\Project\Knowledge-Burger\Picture\通讯\MQTT\MQTT特点.png)

Publisher和Subscriber为客户端，Broker为服务器端，消息主题为消息类型，Broker根据Topic过滤消息，并将消息向客户端推送。

MQTT中用QoS表示服务质量，MQTT协议中有三种服务质量(QoS)：
QoS =0，至多一次，可能会出现丢包的情况，使用在对实时性要求不高的情况，例如，将此服务质量与通信环境传感器数据一起使用。 对于是否丢失个别读取或是否稍后立即发布新的读取并不重要。
QoS =1,至少一次，保证包会到达目的地，但是可能出现重包。
QoS =2, 刚好一次，保证包会到达目的地，且不会出现重包的现象。



# MQTT Broker

**MQTT Broker** 是一种消息中间件，专为物联网（IoT）设计，支持设备间的高效通信。它基于 **发布/订阅** 模型，允许客户端通过主题（Topic）发布或订阅消息。常见的开源 MQTT Broker 包括 **EMQX**、**HiveMQ**、**VerneMQ**、**ActiveMQ** 和 **Mosquitto**

## 1. MQTT服务测试

### 1.1 使用命令行进行测试

#### 1.1.1 订阅主题

通过cmd命令，切换到mosquitto安装目录，打开第一个命令窗口，订阅主题

![](D:\Project\Knowledge-Burger\Picture\通讯\MQTT\订阅主题.png)

#### 1.1.2 发布主题及消息

打开第二个命令窗口，发布主题

![](D:\Project\Knowledge-Burger\Picture\通讯\MQTT\发布主题.png)

#### 1.1.3 打印消息

发布主题后打印的信息

![](D:\Project\Knowledge-Burger\Picture\通讯\MQTT\消息接收.png)