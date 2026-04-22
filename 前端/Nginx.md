# Nginx 说明

Nginx 是一款轻量级的 Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，其特点是占有内存少，并发能力强。









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

