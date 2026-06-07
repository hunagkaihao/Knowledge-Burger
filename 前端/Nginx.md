# Nginx 说明

Nginx 是一款轻量级的 Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，其特点是占有内存少，并发能力强。

## 正向代理

如果我们把google想象成为一个资源库，则大陆局域网的客户端要访问这个资源库，就需要通过代理服务器来访问，这种代理服务就叫做正向代理。
**简单说就是：在客户端（浏览器）配置代理服务器，通过代理服务器进行互联网访问。**

## 反向代理

反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问，我们只需要将请求发送到反向代理服务器，反向代理服务器去选择目标服务器获取数据后，再返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，**暴露的是代理服务器地址，隐藏了真实服务器IP地址。**

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\反向代理.png)

# 负载均衡

当请求变多，单体应用不能满足，我们增加服务器的数量，然后将请求分发到不同服务器上解决高并发，就是负载均衡。

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\负载均衡.png)

### Nginx 负载均衡算法

1. 轮询（Round Robin）

   ```bash
   upstream backend {
       server backend1.example.com;
       server backend2.example.com;
   }

特点：按顺序依次将请求分配给后端服务器，默认算法。
适用场景：后端服务器性能相近的场景。

2. 加权轮询（Weighted Round Robin）

   ```bash
   upstream backend {
       server backend1.example.com weight=5;
       server backend2.example.com weight=2;
   }

特点：根据 weight 参数指定权重，权重越高分配的请求越多。
适用场景：后端服务器性能差异较大时，按性能比例分配请求。

3. IP 哈希（IP Hash）

```bash
upstream backend {
    ip_hash;
    server backend1.example.com;
    server backend2.example.com;
}
```

特点：根据客户端 IP 的哈希值分配服务器，确保同一客户端始终访问同一服务器。
适用场景：需要 session 会话保持的场景（如购物车、登录状态）。

4. 最少连接（Least Connections）

```bash
upstream backend {
    least_conn;
    server backend1.example.com;
    server backend2.example.com;
}
```

特点：将请求分配给当前连接数最少的服务器。
适用场景：处理请求耗时差异较大的场景（如动态内容与静态内容混合）。

5. 加权最少连接（Weighted Least Connections）

```bash
upstream backend {
    least_conn;
    server backend1.example.com weight=5;
    server backend2.example.com weight=2;
}
```

特点：在最少连接的基础上考虑权重，优先选择连接数少且权重高的服务器。
适用场景：结合服务器性能差异和连接状态的场景。

6. 通用哈希（Generic Hash）

```bash
upstream backend {
    hash $request_uri consistent;
    server backend1.example.com;
    server backend2.example.com;
}
```

特点：根据自定义 key（如 URL、用户 ID）的哈希值分配服务器。
参数：
consistent：启用一致性哈希，减少服务器增减时的缓存失效问题。
适用场景：缓存集群、分布式系统中需要固定请求路由的场景。

7. 随机（Random）

```bash
upstream backend {
    random two least_conn;
    server backend1.example.com;
    server backend2.example.com;
}
```

特点：随机选择两台服务器，再根据 least_conn 或 weight 选择最优。
参数：
two：随机选择两台服务器。
least_conn/weight：进一步筛选的策略。
适用场景：需要随机化且兼顾负载的场景。

8. 粘性会话（Sticky Session）

```bash
upstream backend {
    sticky cookie srv_id expires=1h domain=.example.com path=/;
    server backend1.example.com;
    server backend2.example.com;
}
```

特点：通过 Cookie 实现会话保持，需编译 ngx_http_upstream_sticky_module 模块。
适用场景：需要会话保持但不依赖客户端 IP 的场景。

| 算法     | 核心逻辑                 | 使用场景             |
| -------- | ------------------------ | -------------------- |
| 轮询     | 按顺序分配               | 服务器性能相近       |
| 加权轮询 | 按权重比例分配           | 服务器性能差异大     |
| IP 哈希  | 同一 IP 固定到同一服务器 | 需要会话保持         |
| 最少连接 | 优先分配连接数少的服务器 | 请求耗时差异大       |
| 通用哈希 | 自定义 key 哈希路由      | 缓存集群、分布式系统 |
| 随机     | 随机 + 筛选              | 需要随机化的场景     |
| 粘性会话 | 通过 Cookie 固定服务器   | 不依赖 IP 的会话保持 |

## 动静分离

为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速度。降低原来单个服务器的压力。

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\动静分离.png)

# Nginx 下载

#### [1.打开nginx官网](http://nginx.org/en/index.html)

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\Nginx官网.png)

#### 2.点击下载 

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\download.png)

####  3.选择稳定版本（windows）

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\windowNginx.png)

####  4.然后就是解析安装到指定目录下

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\安装.png)

二、启动nginx服务器
1.启动服务器
使用命令提示符进入nginx中，输入一下命令(注意：回车确认是会出现一闪，这是正常现象）：

```bash
start nginx 
```

2、再是查看任务进程是否存在，dos或打开任务管理器都行
我们以命令提示查看方式输入一下命令

```bash
tasklist /fi "imagename eq nginx.exe"
```

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\NginxPid.png)

####  3、最后一步是打开我们的浏览器访问刚才的域名及端口，nginx默认http://localhost:80或127.0.0.1:80，默认端口号是80，出现Welcome to nginx!就说明部署成功了！

![](D:\Project\Knowledge-Burger\Picture\前端\Nginx\启动.png)



# Nginx 快速使用教程

### **📦 第一步：获取打包文件**

1. 在你的项目目录下，找到运行 `npm run build` 后生成的 **`dist`** 文件夹。
2. 这个文件夹里通常只有三个主要部分：`assets` 文件夹、`index.html` 以及其他可能的静态资源。
3. **复制**这个 `dist` 文件夹（或者文件夹里的所有内容）。

------

### **⚙️ 第二步：部署到服务器（以 Nginx 为例）**

Nginx 是目前最流行、最轻量级的静态资源服务器，非常适合部署 Vue/React 前端项目。

#### **下载与安装 Nginx**

- 去 Nginx 官网下载 Windows 版（如果是 Windows 服务器）或 Linux 版。
- 解压到任意目录，例如 `D:\nginx-1.24.0`。

#### **放置文件**

- 打开 Nginx 解压目录，找到 **`html`** 文件夹。
- **清空** `html` 文件夹里的默认文件（如 `index.html`, `50x.html` 等）。
- 将你刚才复制的 `dist` 文件夹里的 **所有内容**，粘贴到这个 `html` 文件夹里。

#### **配置 Nginx（关键步骤）**

因为你的前端项目使用了 Vue Router（看截图有 `router` 文件夹），大概率是 **History 模式**。如果不配置，用户刷新页面时会报 404 错误。

1. 打开 Nginx 目录下的 `conf` 文件夹，用记事本或 VS Code 打开 **`nginx.conf`** 文件。

2. 找到 `server` 块，修改 `location /` 部分，添加 `try_files` 配置：

   ```
   server {
       listen       80;  # 监听端口，默认80
       server_name  localhost; # 你的域名或IP
   
       location / {
           root   html;
           index  index.html index.htm;
           
           # 👇 加上这一行，解决刷新页面 404 的问题
           try_files $uri $uri/ /index.html; 
       }
   
       # 其他配置...
   }
   ```

   #### **启动服务器**

   - 双击 Nginx 目录下的 `nginx.exe`。
   - 或者在命令行输入 `start nginx`。
   - 打开浏览器访问 `http://localhost`（或者是你服务器的 IP 地址），你应该就能看到你的网页了。

#### **方案 A：使用 Nginx 反向代理（推荐）**

既然前端已经部署在 Nginx 上了，我们可以让 Nginx 顺便把 API 请求转发给后端。这样前端代码不需要改任何 IP 地址。

修改 `nginx.conf`，在 `server` 块里增加一段 `location`：

```
server {
    listen       80;
    server_name  localhost;

    # 前端页面配置
    location / {
        root   html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }

    # 👇 新增：API 代理配置
    location /pda-api/ {
        # 这里填你真实的后端 IP 和端口
        proxy_pass http://192.168.68.6:8022/; 
        
        # 以下是一些必要的头信息，保持默认即可
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

**修改后重启 Nginx**（命令行输入 `nginx -s reload`）。

这样，当浏览器请求 `/pda-api/login` 时，Nginx 会自动把它转给 `192.168.68.6:8022/login`，完美解决了跨域问题。

# 问题

1. 替换文件后还是网页还是旧页面

   解决方法：Ctrl + F5

