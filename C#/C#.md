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

# 多线程

`概念`：程序同时执行代码的机制称为多线程

线程是抢占式的



# 同步操作与异步操作

**同步操作**：先完成其工作再返回调用者

**异步操作**：大部分工作则是再返回给调用者之后才完成

# 延续

`概念`：延续会告知任务在完成之后继续执行后续的操作。延续通常由一个回调方法实现，该方法会在操作完成之后执行。

# IDisposable

`IDisposable` 是 .NET 中用于**显式释放非托管资源**（如文件句柄、数据库连接、网络套接字、GDI 对象、COM 组件等）的核心接口。正确使用它能防止内存泄漏、资源耗尽和系统不稳定。

- **托管资源**（如普通对象、数组）由 **垃圾回收器 (GC)** 自动管理。
- 非托管资源（操作系统级别的资源）GC 无法自动释放！
  - 例如：打开的文件、数据库连接、窗口句柄、TCP 连接、未托管内存等。
- 如果不手动释放，会导致：
  - 文件被锁住无法删除
  - 数据库连接池耗尽
  - 内存泄漏（非托管内存）
  - 系统资源枯竭

> ✅ `IDisposable` 的核心目的：**确保非托管资源被及时、可靠地释放**。

# 流式接口和非流式接口

`概念`：流式接口和非流式接口主要区别在于数据传输和处理的方法。

`流式接口`：适用于实时数据传输和逐步处理的场景

1. 数据以连续的小块形式传输
2. 数据在生成后立即发送，客户端可以逐步接收和处理
3. 实时传输，低延迟，数据可以在传输过程中处理

`非流式接口`：适用于数据整体传输和处理的场景

1. 数据作为一个整体进行传输
2. 数据在准备完成后才开始传输，客户端在接收全部数据后才处理
3. 可能会有较高的初始延迟，数据处理开始时需要等待传输完成

# 序列化

`序列化`：是将内存中的对象或者对象图（一组相互引用的对象）拉平为一个可以保存或进行传输的字节流，或者 XML 节点。

`反序列化`：把数据流重新构造成内存中的一个对象或者对象图。

# 反射

`概念`：在运行时检查并使用元数据和编译代码的操作称为反射





# 自定义特性

在 C# 中，**自定义特性 (Custom Attributes)** 是一种强大的元数据机制，允许你将声明性信息（如描述、规则、配置）附加到代码元素（类、方法、属性、字段等）上。这些信息可以在**编译时**被编译器检查，或在**运行时**通过**反射 (Reflection)** 读取并执行相应的逻辑。

**基本模板**

```C#
using System;

// 1. 指定特性可以应用的目标（如类、方法、属性），可选
// 2. 指定是否允许多次应用同一个特性
[AttributeUsage(AttributeTargets.All, AllowMultiple = false)]
public class MyCustomAttribute : Attribute
{
    // 构造函数：用于接收位置参数 (Positional Arguments)
    public string Description { get; }
    public int Priority { get; set; } // 命名参数 (Named Argument)，必须是可读写属性

    public MyCustomAttribute(string description)
    {
        Description = description;
        Priority = 1; // 默认值
    }
}
```
