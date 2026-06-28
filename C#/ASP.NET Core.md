# ASP.NET Core

## 定义

ASP.NET Core 是一系列小的模块化组件，可添加到现有应用中，用于开发 Web 应用和微服务。

跨平台（Windows、Linux、Mac）

## 反向代理

服务器机器上需要一种能够接收请求并提供响应的软件（Kestrel，IIS, Nginx, Docker，Apache），这也被称为反向代理



## Kestrel

Kestrel 是 ASP.NET Core 应用程序的默认跨平台 HTTP 服务器。

它既充当开发服务器，也作为能接收真实互联网请求的实际应用服务器。



![](D:\Project\Knowledge-Burger\Picture\C#\服务器.png)

# ASP.NET Core MVC（Model-View-Controller 模型-视图-控制器）

 不要与早期框架 ASP.NET MVC 混淆，其只适合在 Windows 平台下进行开发





# ASP.NET Core Web API

只包含模型和控制器，但没有视图

通常用于创建 RESTful 服务，以便接收请求并以数据形式返回响应，仅返回数据，不返回视图



# ASP.NET Core Razor Pages

以页面为中心的场景



# ASP.NET Core Blazor

客户端和服务器端都使用 C# 开发应用程序



# 属性

## [FromQuery]

`[FromQuery]` 是一个模型绑定属性（Attribute），它的核心作用是**指示控制器方法从 HTTP 请求的查询字符串（Query String）中提取参数值**。

查询字符串通常位于 URL 中 `?` 后面的部分，由多个键值对组成，并使用 `&` 符号进行分隔。

假设前端发送的请求 URL 为：`/api/users?page=1&name=Alice`

在后端控制器中，你可以这样接收参数：

```C#
[HttpGet("users")]
public IActionResult GetUsers([FromQuery] int page, [FromQuery] string name)
{
    // page 的值为 1
    // name 的值为 "Alice"
    return Ok(new { page, name });
}
```

## [FromBody]

**`[FromBody]`**：数据在 HTTP 请求体（Body）中，通常用于 `POST` 或 `PUT` 请求，适合传递复杂的 JSON 对象或大量数据。

**适用场景**：POST/PUT 请求，创建或更新复杂对象。

**前端请求**：

```http
POST /api/products
Content-Type: application/json

{ "name": "Laptop", "price": 999.99 }
```

**后端代码**：

```C#
[HttpPost("products")]
public IActionResult CreateProduct([FromBody] Product product)
{
    // product.Name = "Laptop"
    // product.Price = 999.99
    return Ok(product);
}
```
