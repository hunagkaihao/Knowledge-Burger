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

# 线程

线程是一个可以独立执行的执行路径。

## 使用场景

需要长期运行、独立、后台、不归还线程池的线程

## 特定

- 独立线程，不进入线程池
- 不自动释放，一直运行
- 开销大（约 1MB 栈内存 + 系统资源）
- 适合长期运行
- 不适合临时任务

# 线程池

每当启动一个线程时，都需要一定的时间（几百毫秒）来创建新的局部变量栈。而线程池通过预先创建一个可回收线程的池子来降低这个开销。线程池对开发高性能的并行程序和细粒度的并发都是非常必要的。它可以支持运行一些短暂的操作而不会受到线程启动开销的影响。

## 特点

1. 线程复用：不用每次都创建/销毁，大大提升性能
2. 资源消耗低：创建一个 Thread 约 1MB 内存。线程池用现成线程，几乎无开销
3. 系统自动管理：自动调节数量、避免 CPU 爆炸
4. 简单易用：Task.Run/async/await 全部基于线程池

# 任务

它代表了一个并发操作，而该操作并不一定依赖线程来完成。

## 使用场景

临时任务、短时任务、计算任务、不长期占用

## 特点

- 来自线程池

- 用完自动归还线程池

- 非常轻量

- 系统自动管理、自动复用

- 支持 async/await 

- 代码简洁

  

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

# 委托

`概念`：委托是一种知道如何调用方法的对象。

## 委托实例

委托实例字面上是调用者的代理：调用者调用委托，而委托调用目标方法。这种间接调用的方式可以将调用者和目标方法解耦。

# 事件

`概念`：事件是一种使用有限的委托功能实现广播者/订阅者模型的结构。

使用事件的主要目的在于保证订阅者之间不互相影响。

# 委托和事件的区别

委托 = 可以被随便调用、赋值、覆盖的”函数指针“

事件 = 加了安全锁、只能 += / -=、不能外部触发的”委托封装“

# 反射

`概念`：在运行时检查并使用元数据和编译代码的操作称为反射

`用途`：

1. 动态创建对象
2. 动态获取/修改属性/字段值
3. 动态调用方法
4. 读取特性

# Array类

Array 类是所有一维和多维数组的隐式基类，它是实现标准集合接口的最基本类型之一。

## 特点

- 固定长度，一旦创建不能变
- 连续内存，访问最快
- 类型安全
- 只能用 [] 访问
- 适合：长度固定、高性能、大量数据

# List

## 特点

- 动态长度，自动扩容
- 底层本质是 T[]
- 提供 Add / Remove / Find / Foreach 等方法
- 比数组好用、灵活
- 适合：日常开发 90% 场景



# 字典

字典是一种集合，其包含的元素均为键值对。字典通常用于查找或用作排序列表。

## 底层原理

Dictionary = 哈希表（Hash Table）

通过 哈希函数 把 key 转成小标，实现 O（1）查找。

**底层结构**

- 一个 哈希桶数组 （bucket）
- 一个 碰撞链表（解决哈希冲突）
- 一个 entries 数组 存实际数据

**工作流程**

1. 对 Key 计算 Hash Code
2. 取模得到 桶索引
3. 找到对应位置
4. 如果冲突，用 链表 串起来

**特点**

- 查找超快 O（1）
- 插入删除快
- 无序
- 键不能重复

## ConcurrentDictionary

使用场景：多线程同时读写集合时

# 全局异常捕获

### 1. **AppDomain.CurrentDomain.UnhandledException**

捕获 **所有线程** 的异常，终极兜底。

### 2. **TaskScheduler.UnobservedTaskException**

捕获 **异步 Task 未观察的异常**（最容易闪退）

```C#
using System;
using System.Threading.Tasks;

public static class GlobalExceptionHandler
{
    public static void Register()
    {
        // 1. 捕获所有线程异常（终极兜底）
        AppDomain.CurrentDomain.UnhandledException += (sender, e) =>
        {
            Exception ex = e.ExceptionObject as Exception;
            HandleException(ex, "AppDomain Unhandled Exception");
        };

        // 2. 捕获异步 Task 未观察异常
        TaskScheduler.UnobservedTaskException += (sender, e) =>
        {
            Exception ex = e.Exception;
            HandleException(ex, "Task Unobserved Exception");
            e.SetObserved(); // 标记异常已处理，不闪退
        };
    }

    // 统一处理异常：记录日志 + 保存 + 安全退出
    private static void HandleException(Exception ex, string type)
    {
        try
        {
            string log = $"==================== 异常 ====================\r\n" +
                         $"类型：{type}\r\n" +
                         $"时间：{DateTime.Now:yyyy-MM-dd HH:mm:ss}\r\n" +
                         $"异常：{ex.Message}\r\n" +
                         $"堆栈：{ex.StackTrace}\r\n" +
                         $"==============================================\r\n\r\n";

            // 保存日志
            string path = AppDomain.CurrentDomain.BaseDirectory + "ErrorLog.txt";
            System.IO.File.AppendAllText(path, log);
        }
        catch
        {
            // 日志写入失败也不能再抛异常
        }

        // 这里可以做：
        // 关闭CAN
        // 停止电机
        // 保存配置
        // 弹窗提示
    }
}
```

# null 合并运算符：??

左值 ?? 右值：如果左值不为null,则为左值；反之为右值

```c#
// 示例
int x = null
int y = x ?? 5;
Console.Write(y);// 5
```

---

# null 条件运算符：?.

左值 ?. 成员或方法：当运算符的左侧为null的时候，该表达式的运算结果也是null而不会抛出NullReferenceException异常。

```C#
// 示例
System.Text.StringBuilder sb = null;
string s = sb?.ToString();
Console.Write(s);// null
```

---

# Substring

- ***Substring(int startIndex)\***：从*startIndex*位置开始截取直到字符串的末尾。

- ***Substring(int startIndex, int length)\***：从*startIndex*位置开始截取，长度为*length*的子字符串。

  ```C#
  string original = "Hello, World!";
  string result;
  // 从第一个字符开始，截取5个字符
  result = original.Substring(0, 5); // 结果: "Hello"
  Console.WriteLine(result);
  // 从第8个字符开始截取到字符串的末尾
  result = original.Substring(7); // 结果: "World!"
  Console.WriteLine(result);
  ```

---

# 装箱和拆箱

`装箱`：将值类型实例转换为引用类型实例的行为。

`拆箱`：将引用类型实例转换成原始的值类型实例的行为。

# 值类型和引用类型

`值类型`：包含大多数的内置类型（具体包括所有数值类型、char 类型和 bool 类型）以及自定的 struct 类型和 enum 类型。

`引用类型`：包含所有的类、数组、委托和接口类型。（这其中包括了预定义的 string 类型。）

值类型和引用类型的区别：是否含有对象头；值类型声明在栈上，引用类型声明在堆上。                               

# 散列表

即一些使用键来存储和获取元素的集合。

# 协变

假定 A 可以转换为 B，如果 X\<A> 可以转换为 X\<B> 那么称 X 有一个协变类型参数。

# LINQ 查询

LINQ 是 Language Integrated Query 的缩写，它可以视为一组语言和框架特性的集合。

`Take`：输出前 X 个元素，而丢弃其他元素

`Skip`：跳过集合中的前 X 个元素而输出剩余的元素

`Reverse`：将集合中的所有元素反转

`into`：可以在映射之后 “继续” 执行后续查询（into 关键字只能够出现在 select 和 group 子句之后）

`let`：可以在查询中定义一个新的变量，这个新的变量能够和范围变量并存

`Where`：返回输入序列中满足给定断言的那些元素





# LINQ to SQL（L2S）



# Entity data Model （EDM)



# 对象服务（Object Services)

**概念**：能够对该概念模型进行查询和更新操作的类型统称为对象服务



# 表达式树

`表达式树`是一个微型的代码 DOM（文档结构模型）。树中的每一个节点都代表了 System.Linq.Expression 命名空间下的一个类型。



# 文档对象模型（document object model 或 DOM）

`概念`：使用集合属性来存储子内容，用一棵对象树来完整地表示整个文档



# XML DOM（X-DOM）

![](D:\Project\Knowledge-Burger\Picture\C#\X-DOM 核心类型.png)

# IO 密集

`概念`：如果一个操作的绝大部分时间都在等待事件的发生，则称为 I/O 密集



# 计算密集

`概念`：如果操作的大部分时间都用于执行大量的 CPU 操作，则称为计算密集



# 信号发送

`概念`：有时一个线程需要等待来自其他线程的通知，即所谓的信号发送（signaling



# PipeStream

`匿名管道（速度更快）`：支持在同一个计算机中的父进程和子进程之间进行单向通信

`命名管道（更加灵活）`：允许同一台计算机的任意两个进程之间，或者不同计算机（使用 Windows 网络）的两个进程间进行双向同通信



# 等待

await 关键字可以简便地附加延续。

async 修饰符会知识编译器将 await 作为一个关键字而非标识符来避免二义性。其只支持返回类型为 void 以及 Task 或 Task\<TResult>的方法。

**async/await 是语法糖，底层由编译器生成状态机，实现 “看似同步、实则异步” 的非阻塞代码。**

**执行流程**

1. 遇到 `await`
2. **当前线程立即返回，不阻塞**
3. 后台执行异步操作（IO / 网络 / 延时等）
4. 操作完成后
5. **自动切回原来的上下文（UI 线程 / 调用线程）**
6. 继续执行后面代码

**关键点**

- **不会阻塞线程**
- **不卡界面**
- **代码像同步一样好读**
- **底层是 Task + 状态机**

# 排他锁

## lock语句

每一次只能有一个线程锁定同步对象，而其他线程则被阻塞，直至锁释放。

## Mutex

Mutex 和 C# 的 lock 类似，但是它可以支持多个进程。换言之，Mutex 不但可以用于应用程序范围，还可以用于计算机范围。在非竞争的情况下获得或者释放 Mutex 需要大约一微秒的时间，大概比 lock 要慢 20 倍。

Mutex 类的 WaitOne 方法将获得该锁，ReleaseMutex 方法将释放该锁。Mutex 只能在获得锁的线程中释放锁。

# 非排他锁

## 信号量

控制最多 N 个线程同时进入，不是独占锁，是限流。

```c#
SemaphoreSlim sem = new SemaphoreSlim(3); // 最多3个线程
```

# 死锁

两个线程互相等待对方占用的资源就会使双方都无法继续执行，从而形成死锁。

# CRUD

CRUD 是四个英文单词的缩写：**C**reate（创建）、**R**ead（读取）、**U**pdate（更新）、**D**elete（删除）。

# IsLetter

`IsLetter` 是 C# 中一个非常实用的**字符检查方法**，用于判断一个字符是否为 Unicode 字母。

```C#
char c = 'A';
bool result = char.IsLetter(c);  // result = true
```

