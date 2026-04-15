# Swashbuckle.AspNetCore 使用教程

### **🛠️ 第一步：创建项目**

1. 打开 Visual Studio 2022。
2. 点击 **“创建新项目”**。
3. 在搜索框输入 `API`，选择 **“ASP.NET Core Web API”**（注意图标是紫色的，不要选成空的 Web 应用程序），点击“下一步”。
4. **项目名称**：输入 `SwaggerDemo`。
5. **位置**：选一个你方便的文件夹。
6. **框架**：选择 **“.NET 8.0 (长期支持)”**。
7. 点击 **“创建”**。

> **注意**：VS 2022 创建 .NET 8 的 API 项目时，默认模板**可能不会**自动开启 Swagger。如果创建好后直接运行报 404 或者找不到页面，请继续往下看“手动配置”环节，这是面试和实战中必须掌握的技能。

------

### **📦 第二步：安装 Swashbuckle 包**

虽然 .NET 6/8 有内置支持，但显式安装 `Swashbuckle.AspNetCore` 是最稳妥、最标准的做法。

1. 在“解决方案资源管理器”中，**右键点击** `SwaggerDemo` 项目。
2. 选择 **“管理 NuGet 程序包”**。
3. 点击 **“浏览”** 选项卡。
4. 搜索 `Swashbuckle.AspNetCore`。
5. 找到由 Microsoft 发布的这个包，点击 **“安装”**。
6. 点击“确定”接受许可。

------

### **⚙️ 第三步：修改 Program.cs（核心步骤）**

这是最关键的一步。打开项目根目录下的 `Program.cs` 文件。你需要确保代码里包含以下两部分配置。

为了演示效果，我直接把完整的 `Program.cs` 贴给你，你可以对照修改：

```C#
using Microsoft.OpenApi.Models; // 1. 引入命名空间，用于配置 Swagger 信息
using System.Reflection;       // 2. 引入命名空间，用于获取 XML 注释

var builder = WebApplication.CreateBuilder(args);

// --- 服务配置区域 ---

// 添加控制器服务
builder.Services.AddControllers();

// 3. 【关键】添加 Swagger 服务
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(c =>
{
    // 设置 Swagger 文档的基本信息（显示在网页左上角）
    c.SwaggerDoc("v1", new OpenApiInfo 
    { 
        Title = "我的可视化 API 演示", 
        Version = "v1",
        Description = "这是一个用于演示 Swashbuckle 的简单项目"
    });

    // 4. 【可选但推荐】开启 XML 注释支持
    // 这样你在代码里写的 /// 摘要 就能显示在网页上
    var xmlFile = $"{Assembly.GetExecutingAssembly().GetName().Name}.xml";
    var xmlPath = Path.Combine(AppContext.BaseDirectory, xmlFile);
    if (File.Exists(xmlPath))
    {
        c.IncludeXmlComments(xmlPath);
    }
});

var app = builder.Build();

// --- 管道配置区域 ---

// 配置 HTTP 重定向
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

// 5. 【关键】启用 Swagger 中间件
// 建议只在开发环境开启，或者生产环境加权限验证
if (app.Environment.IsDevelopment())
{
    app.UseSwagger(); // 生成 JSON 文档
    
    app.UseSwaggerUI(c =>
    {
        // 配置 UI
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
        
        // 设置为根路径访问，这样运行后直接看到页面，不用加 /swagger
        c.RoutePrefix = string.Empty; 
    });
}

app.Run();
```

------

### **⚙️ 第四步：开启 XML 注释（让文档更漂亮）**

为了让 Swagger 界面显示中文注释（比如“获取天气信息”），而不是只显示冷冰冰的接口名，我们需要开启 XML 文档生成。

1. 在解决方案资源管理器中，**右键点击** 项目 `SwaggerDemo`。
2. 选择 **“属性”**。
3. 在左侧选择 **“生成”** (Build)。
4. 在右侧找到 **“输出”** 部分。
5. 勾选 **“XML 文档文件”**。
6. 保存（Ctrl+S）。

------

### **🧪 第五步：编写一个测试接口**

为了验证效果，我们修改 `WeatherForecastController.cs`，或者新建一个控制器。这里直接修改默认的控制器：

```C#
using Microsoft.AspNetCore.Mvc;

namespace SwaggerDemo.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        /// <summary>
        /// 获取天气预报列表
        /// </summary>
        /// <param name="count">返回的数量</param>
        /// <returns>返回一组天气数据</returns>
        [HttpGet(Name = "GetWeatherForecast")]
        public IEnumerable<WeatherForecast> Get(int count = 5)
        {
            return Enumerable.Range(1, count).Select(index => new WeatherForecast
            {
                Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                TemperatureC = Random.Shared.Next(-20, 55),
                Summary = Summaries[Random.Shared.Next(Summaries.Length)]
            })
            .ToArray();
        }
    }
}
```

------

### **🚀 第六步：运行并查看效果**

1. 按 **F5** 或点击顶部的绿色播放按钮（IIS Express 或 SwaggerDemo）。
2. 浏览器会自动打开。
3. **如果你设置了 `c.RoutePrefix = string.Empty;`**，你应该直接看到一个白底的网页，标题是“我的可视化 API 演示”。
4. 如果你没有设置根路径，网址应该是 `https://localhost:xxxx/swagger`。

**你将看到：**

- 一个名为 `WeatherForecast` 的接口组。
- 点击 `GET`，展开后能看到你刚才写的中文摘要：“获取天气预报列表”。
- 点击右下角的 **“Try it out”**。
- 点击 **“Execute”**。
- 下方会显示 **Server response**，状态码 200，以及返回的 JSON 数据。

### **📌 总结**

这就完成了！

- **AddSwaggerGen**：负责“写文档”（生成 JSON）。
- **UseSwaggerUI**：负责“看文档”（渲染网页）。
- **XML 注释**：负责“说人话”（显示中文说明）。