***静态绑定***：编译器找到对应匹配的方法

## 富客户端

即用户必须下载并安装到电脑或移动设备上的程序

## 瘦客户端

例如网站

## MvC (Model-View-Controller)

## WPF (Windows Presentation Foundation)

## UWP（Universal Windows Platform)

## ADO.NET

`ADO (ActiveX Data Object)`

- 提供者层（Provider Layer)：提供者模型定义了对数据库提供者进行底层访问的通用类和接口。
- DataSet 模型：数据的结构化缓存。

## ORM（object relational mappers)

自动将对象（基于用户定义的类）映射到数据库的记录行。

## EF Core（Entity Framework Core)

## WCF（Windows Communication Foundation)
`分布式系统技术`

- 在服务器端，可以指定远程客户端应用程序能够调用的方法
- 在客户端，可以指定或推断将要调用的服务器方法的签名
- 在服务器和客户端，都可以选择一种传输和通信协议（在WCF 中，这是通过使用绑定实现的）
- 客户端和服务器建立连接
- 客户端调用远程方法，并在服务器上透明地执行

## SOAP (简单对象访问协议，Simple Object Access Protocol)

## Web API

`概要`:Web API 运行于 ASP.Net/ASP.NET Core 之上，在架构上和 Microsoft 的 MVC API 相似，只不过 Web API 不是为生产 Web 页面，而是为提供服务和数据进行设计的。

#### 创建字符串

```C#
// 创建一个重复的字符序列
Console.WriteLine(new string('*', 10));// **********

// 从 char 数组来构造一个字符串
char[] ca = "Hello World".ToCharArray();
string s = new string (ca);
Console.WriteLine(s);// Hello World
```

#### 字符串处理

`Substring`：提取部份字符串

`insert`：插入一些字符

`Remove`：删除一些字符

`PadLeft 和 PadRight`：用特定的字符（如果未指定则使用空格）将字符串填充为指定的长度

`TrimStart 和 TrimEnd`：从字符串的开始或结尾删除指定的字符

`Trim`：从开始和结尾执行删除操作

`Replace`：替换字符串中所有（费重叠的）特定字符或子字符串；

#### 序列比较与文化相关的字符串比较

`序列比较（ordinal)`：直接将字符串解析为数字（按照它们的 Unicode 字符数值）

`文化相关的比较`：参照特定的字母表来解析字符

### StringBulider

### 文本编码和 Unicode

`ASCII`：只是 Unicode 字符集的前 128 个字符

`Unicode`：约一百万个字符的地址空间

`文本编码（text encoding)`：将字符从器数字代码点映射到二进制表示的方法



***BMP (基本多文种平面 Basic Multilingual Plane)***：包括了 84 种世界范围内的语言，并含有超过 30 000 个汉字字符。只有一些古代语言、乐谱符号和生僻汉字字符不包含在内。



### TimeSpan

表示一段时间间隔或是一天内的时间

### DateTime 和 DateTimeOffset

表示日期或事件的不可变结构体

UTC (现代的格林尼治时间)

Update-Database



