# 概要

Docker 是一种工具，它能轻松地实现为任何应用程序创建可部署的软件包的功能，还能将这些软件包轻松部署到各种环境中。此外，它还能简化敏捷软件开发团队的工作流程，提升其响应速度。

### **第一阶段：理解“发布”**

在本地开发时，我们用的是 `dotnet run`，它依赖你电脑上安装的 SDK。但在服务器上，我们通常只放编译好的文件。

#### **核心命令**

在终端中运行：

```
dotnet publish -c Release -o ./publish_output
```

- `-c Release`：发布“发布”模式（代码经过优化，运行更快，不包含调试信息）。
- `-o`：指定输出目录。

#### **观察成果**

打开 `publish_output` 文件夹，你会看到：

- `.dll` 文件（你的程序）。
- `.deps.json` 和 `.runtimeconfig.json`（告诉 .NET 运行时需要哪些依赖）。

> **知识点**：这就是所谓的“依赖框架部署”。前提是服务器上必须安装了对应版本的 .NET 运行时。如果想连运行时一起打包，就需要“独立部署”，但体积会很大。

------

### **第二阶段：Docker 容器化**

这是现代开发的重中之重。Docker 就像一个“集装箱”，把你的应用和它需要的所有环境（Linux、.NET 运行时）打包在一起，走到哪里都能跑。

#### **编写 Dockerfile**

在你的项目根目录（和 `.csproj` 同级）创建一个名为 `Dockerfile` 的文件（没有后缀名）。

**推荐模板（多阶段构建）：**
这种写法可以让最终的镜像非常小。

```
# 第一阶段：构建阶段
# 使用包含 SDK 的镜像，用来编译代码
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
# 还原依赖并编译发布
RUN dotnet publish "SwaggerDemo.csproj" -c Release -o /app/publish

# 第二阶段：运行阶段
# 使用体积更小的运行时镜像，只用来跑程序
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
# 从构建阶段拷贝发布好的文件过来
COPY --from=build /app/publish .

# 暴露端口（.NET 8 默认是 8080 或 80）
EXPOSE 8080

# 启动命令
ENTRYPOINT ["dotnet", "SwaggerDemo.dll"]
```

#### **构建与运行**

在终端执行以下命令：

1. 构建镜像

   （注意最后的 . 代表当前目录）：

   ```dockerfile
   # docker build -t 镜像名 .
   docker build -t my-swagger-api .
   ```

2. 运行容器：

   ```
   # -d: 后台运行, -p: 端口映射 (宿主机:容器)
   docker run -d -p 5116:8080 --name my_running_api my-swagger-api
   ```

现在，打开浏览器访问 `http://localhost:5116/swagger`，如果你看到了接口文档，恭喜你，部署成功！

ASP.NET Core 的官方镜像默认监听 **8080** (HTTP) 和 **8443** (HTTPS)



### 问题排查

当遇到容器异常停止时，最有效的排查方法是查看容器的日志。

1. 找到已停止的容器 ID

   ```
   docker ps -a
   ```

2. 查看该容器的日志

   ```
   docker logs <你的容器ID或名称>
   ```

   日志通常会直接告诉你应用为什么启动失败，例如“配置文件未找到”、“无法连接到数据库”或“内存溢出”等关键错误信息。

### **最佳解决方案：环境感知配置**

我们需要写一段代码，**自动判断**当前是在 Docker 里还是本地。

- 如果在 **Docker (Production)**：强制把 Swagger 放在根路径 `/`。
- 如果在 **本地 (Development)**：保留默认的 `/swagger` 路径，避开 HTTPS 重定向的坑。

请修改你的 `Program.cs` 中的 `UseSwaggerUI` 部分：

```C#
if (app.Environment.IsDevelopment())
{
    // 🟢 本地开发环境：使用默认路径 /swagger
    // 这样不会触发 HTTPS 重定向冲突，访问 localhost:5116/swagger 即可
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
        // 这里保持注释或设为 "swagger"，不要设为 Empty
        // c.RoutePrefix = string.Empty; 
    });
}
else
{
    // 🔵 生产/Docker 环境：强制放在根路径 /
    // 这样 Nginx 或浏览器直接访问 IP 就能看到文档
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
        c.RoutePrefix = string.Empty; // 关键：占领根路径
    });
}
```