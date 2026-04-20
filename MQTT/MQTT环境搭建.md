# MQTT 环境搭建

## EMQX 安装流程

1. 下载 EMQX

   [Directory listing for EMQX: /v5.3.2/ | EMQ](https://www.emqx.com/zh/downloads/broker/v5.3.2)

   EMQX从5.4.0版本起，不再支持windows系统，所以我们选5.3.2的windows 版本进行下载。

2. 启动 EMQX

   解压安装包。在bin目录下启动控制台，在控制台里输入  .\emqx start，启动emqx

3. 登录 EMQX 控制台

   打开浏览器，输入localhost:18083,如果出现EMQX控制台画面，说明安装成功。

   默认用户名为 admin，密码为public， 登录后，会要求修改密码，改完的密码最好记录下来，别忘记了（新密码：tt888888）。

4. 登录后，进入EMQX控制台，页面如下

   ![](D:\Project\Knowledge-Burger\Picture\通讯\MQTT\EMQX.png)

## MQTT 客户端安装

#### **1. 下载并安装 MQTTX**

- 访问 MQTTX 官网：[https://mqttx.app/zh](https://mqttx.app/zh?spm=5176.28103460.0.0.96a029888OMgZM)
- 根据你的操作系统（Windows / macOS / Linux）下载对应的安装包。
- 安装过程非常简单，一路“下一步”即可。

#### **2. 使用 MQTTX 连接到你的本地服务器**

1. 打开 MQTTX 应用。
2. 点击左上角的 **“+ New Connection”**（新建连接）。
3. 在配置页面中填写以下信息：
   - **Name**: 给你的连接起个名字，比如 `Local EMQX`。
   - **Client ID**: 可以留空，MQTTX 会自动生成一个。
   - **Host**: `localhost` （因为服务器就在你本机）
   - **Port**: `1883` （这是 MQTT 的默认非加密端口）
   - **Username**: `admin` （如果你的 EMQX 启用了认证）
   - **Password**: `public`
4. 点击右上角的 **“Connect”**（连接）按钮。如果一切正常，你会看到连接成功的状态。
