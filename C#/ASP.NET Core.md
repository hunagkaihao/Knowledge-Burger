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
