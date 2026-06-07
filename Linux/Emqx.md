# EMQX 安装

在 Ubuntu 虚拟机上部署 EMQX，推荐使用官方提供的 **Apt 源安装**方式，这是最稳定、最易维护的方法。以下是完整的部署步骤：

### **第一步：添加 EMQX 官方 APT 仓库**

打开终端，执行以下命令，将 EMQX 的软件源添加到你的系统中：

```
curl -s https://assets.emqx.com/scripts/install-emqx-deb.sh | sudo bash
```

这个脚本会自动配置好 EMQX 的 GPG 密钥和软件源地址。

### **第二步：安装 EMQX**

执行以下命令开始安装：

```
sudo apt-get install emqx
```

系统会自动下载并安装 EMQX 及其依赖项（如 Erlang 运行时）。

### **第三步：启动并设置开机自启**

安装完成后，使用以下命令启动服务并设置为开机自动运行：

```
sudo systemctl start emqx
sudo systemctl enable emqx
```

### **第四步：验证服务状态**

检查 EMQX 是否成功运行：

```
sudo systemctl status emqx
```

如果看到 `active (running)`，说明服务已成功启动。

### **第五步：访问 Web 管理控制台**

EMQX 提供了一个功能强大的图形化管理界面（Dashboard），默认监听端口为 `18083`。

在你的 Windows 宿主机浏览器中访问：
`http://<你的Ubuntu虚拟机IP>:18083`

- **默认用户名**：`admin`
- **默认密码**：`public`

> 注意：首次登录后，系统会强制要求你修改默认密码。