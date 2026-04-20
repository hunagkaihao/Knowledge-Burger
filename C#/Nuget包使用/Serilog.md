# Serilog 使用教程

### **第一步：集成 Serilog (结构化日志)**

**目标**：不再使用 `Console.WriteLine`，而是记录包含时间、级别、消息和异常详情的结构化数据，并输出到控制台和文件。

#### **1. 安装 NuGet 包**

在项目中安装以下包：

- `Serilog.AspNetCore` (核心集成包)
- `Serilog.Sinks.Console` (输出到控制台)
- `Serilog.Sinks.File` (输出到文件)

#### **2. 修改** `Program.cs`

我们需要在 `Program.cs` 的最顶部引入 Serilog，并配置它。

```C#
using Serilog; // 1. 引入命名空间

var builder = WebApplication.CreateBuilder(args);

// 2. 配置 Serilog
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Information() // 最小级别：Information
    .WriteTo.Console()          // 输出到控制台
    .WriteTo.File("logs/log-.txt", // 输出到文件，按天滚动
        rollingInterval: RollingInterval.Day,
        outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level:u3}] {Message:lj}{NewLine}{Exception}")
    .CreateLogger();

// 3. 将 Serilog 注入到 ASP.NET Core 的日志系统中
builder.Host.UseSerilog(); 

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// ... 中间件配置 ...

app.Run();
```

#### **4. 验证效果**

运行项目后，访问任意接口。你会发现：

- 控制台会有彩色日志输出。
- 项目根目录下会生成 `logs` 文件夹，里面有 `.txt` 日志文件。
- 日志格式包含时间戳、级别（INFO/WARN）、消息。

------

### **第二步：全局异常处理中间件**

**目标**：当代码抛出未捕获异常时，拦截它，记录日志，并返回一个干净的 JSON 格式（而不是丑陋的 HTML 堆栈信息）。

#### **1. 创建中间件类**

在项目中新建一个文件夹 `Middleware`，并添加类 `ExceptionHandlingMiddleware.cs`。

```C#
using System.Net;
using System.Text.Json;

namespace SwaggerDemo.Middleware;

public class ExceptionHandlingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ExceptionHandlingMiddleware> _logger;

    public ExceptionHandlingMiddleware(RequestDelegate next, ILogger<ExceptionHandlingMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            // 1. 记录异常 (使用 Serilog)
            _logger.LogError(ex, "发生未处理的异常: {Message}", ex.Message);

            // 2. 统一返回格式
            context.Response.ContentType = "application/json";
            context.Response.StatusCode = (int)HttpStatusCode.InternalServerError;

            var response = new
            {
                statusCode = 500,
                message = "服务器内部错误，请稍后再试。", // 不暴露具体错误细节给前端
                // details: ex.Message // 开发环境可以打开，生产环境建议关闭
            };

            var json = JsonSerializer.Serialize(response);
            await context.Response.WriteAsync(json);
        }
    }
}
```

#### **2. 在** `Program.cs` **中注册中间件**

中间件必须放在管道的前端，才能捕获后续所有组件的异常。

```C#
using SwaggerDemo.Middleware; // 引入命名空间

var app = builder.Build();

// ... Swagger 配置 ...

// 3. 使用自定义异常中间件 (必须在 app.UseAuthorization() 之前)
app.UseMiddleware<ExceptionHandlingMiddleware>();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

#### **3. 测试**

在任意 Controller 中故意写一个会报错的代码（例如除以零）：

```c#
[HttpGet("crash")]
public IActionResult CrashTest()
{
    int a = 10;
    int b = 0;
    var result = a / b; // 故意崩溃
    return Ok(result);
}
```

**预期结果**：

- 浏览器收到 JSON：`{ "statusCode": 500, "message": "服务器内部错误..." }`
- 日志文件中记录了完整的堆栈跟踪信息。

------

### **第三步：健康检查**

**目标**：提供一个轻量级接口，用于 Docker 或 K8s 判断服务是否存活。

#### **1. 启用健康检查**

在 .NET 6/7/8 中，这非常简单。在 `Program.cs` 中添加：

```C#
// 在 builder.Services 中添加
builder.Services.AddHealthChecks();

// ...

// 在 app 管道中添加映射
app.MapHealthChecks("/health");
```

#### **2. 测试**

运行项目，访问 `http://localhost:5116/health`。

- 如果服务正常，返回 **200 OK** (内容为 "Healthy")。
- 如果服务挂了，接口直接无法访问。