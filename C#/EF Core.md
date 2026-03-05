`EF Core 学习网址`：[入门 - EF Core | Microsoft Learn](https://learn.microsoft.com/zh-cn/ef/core/get-started/overview/first-app?tabs=visual-studio)

# EF Core（Entity Framework Core）





# Razor Pages

### Pages 文件夹

包含 Razor 页面和支持文件。 每个 Razor 页面都是一对文件：

- `.cshtml`文件包含HTML标记和C#代码，使用Razor语法。
- 一个 `.cshtml.cs` 文件，其中包含处理页面事件的 C# 代码。

支持文件的名称以下划线开头。 例如，`_Layout.cshtml` 文件可配置所有页面通用的 UI 元素。 `_Layout.cshtml` 设置页面顶部的导航菜单和页面底部的版权声明。 有关详细信息，请参阅 [ASP.NET Core 中的布局](https://learn.microsoft.com/zh-cn/aspnet/core/mvc/views/layout?view=aspnetcore-10.0)。



### wwwroot 文件夹

包含静态资产，如 HTML 文件、JavaScript 文件和 CSS 文件。 有关详细信息，请参阅 [ASP.NET Core 中的静态文件](https://learn.microsoft.com/zh-cn/aspnet/core/fundamentals/static-files?view=aspnetcore-10.0)。



### `appsettings.json`

包含配置数据，如连接字符串。 有关详细信息，请参阅 [ASP.NET Core 中的配置](https://learn.microsoft.com/zh-cn/aspnet/core/fundamentals/configuration/?view=aspnetcore-10.0)。



---

---

---

# ASP.NET Core 中间件

中间件是一种装配到应用管道以处理请求和响应的软件。 每个中间件：

- 选择是否将请求传递到管道中的下一个中间件。
- 可以在管道中的下一个中间件之前和之后执行工作。

请求委托用于生成请求管道。 请求委托处理每个 HTTP 请求。

使用 [Run](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.builder.runextensions.run)、[Map](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.builder.mapextensions.map) 和 [Use](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.builder.useextensions.use) 扩展方法来配置请求委托。 可以将单个请求委托指定为匿名方法（称为内联中间件），也可以在可重用类中定义。 这些内联匿名方法或可重用类称为 *中间件* 或 *中间件组件*。 请求管道中的每个中间件都负责调用管道中的下一个中间件或对管道进行短路。 当中间件短路时，它被称为“终端中间件”，因为它阻止中间件进一步处理请求。