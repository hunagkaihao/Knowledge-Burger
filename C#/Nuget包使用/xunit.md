# xunit 库的使用

#### **1. 被测试的代码 (Service)**

在你的主项目中，有一个 `WeatherService`：

```C#
// WeatherService.cs
public class WeatherService
{
    public string GetWeatherDescription(int temperature)
    {
        if (temperature > 30)
            return "Hot";
        else if (temperature > 20)
            return "Comfortable";
        else
            return "Cold";
    }
}
```

#### **2. 编写单元测试**

在 `YourProjectName.Tests` 项目中，创建一个 `WeatherServiceTests.cs`。

```C#
// WeatherServiceTests.cs
using Xunit;
using YourProjectName.Services; // 引用主项目命名空间

public class WeatherServiceTests
{
    private readonly WeatherService _service;

    // 构造函数：初始化测试环境
    public WeatherServiceTests()
    {
        _service = new WeatherService();
    }

    // 测试用例 1：测试高温情况
    [Fact] // Fact 表示这是一个测试方法
    public void GetWeatherDescription_WhenTempIs35_ReturnsHot()
    {
        // Arrange (准备)：准备输入数据
        var temp = 35;

        // Act (执行)：调用被测试的方法
        var result = _service.GetWeatherDescription(temp);

        // Assert (断言)：验证结果是否符合预期
        Assert.Equal("Hot", result);
    }

    // 测试用例 2：测试低温情况
    [Fact]
    public void GetWeatherDescription_WhenTempIs15_ReturnsCold()
    {
        // Arrange
        var temp = 15;

        // Act
        var result = _service.GetWeatherDescription(temp);

        // Assert
        Assert.Equal("Cold", result);
    }
}
```

#### **3. 如何运行**

在终端进入测试项目目录，运行：

```
dotnet test
```

#### **检查 .csproj 文件（关键！）**

安装 Microsoft.NET.Test.Sdk、xunit、 xunit.runner.visualstudio

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework> <!-- 版本要和主项目一致 -->
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <IsPackable>false</IsPackable> <!-- 加上这个 -->
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.6.0" />
    <PackageReference Include="xunit" Version="2.4.2" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.4.5" />
  </ItemGroup>

  <ItemGroup>
    <!-- 必须引用你的主项目 -->
    <ProjectReference Include="..\SwaggerDemo\SwaggerDemo.csproj" />
  </ItemGroup>

</Project>
```



### **核心概念：三大支柱**

你在代码中看到的 `Arrange`、`Act`、`Assert` 是单元测试的标准模式，称为 **AAA 模式**。

1. Arrange（准备）：
   - 做什么：准备测试数据、初始化对象、设置 Mock（模拟对象）。
   - 你的代码：`var temp = 35;`
2. Act（执行）：
   - 做什么：调用你要测试的那个方法。
   - 你的代码：`var result = _service.GetWeatherDescription(temp);`
3. Assert（断言）：
   - 做什么：验证结果是否符合预期。如果断言失败，测试就失败。
   - 你的代码：`Assert.Equal("Hot", result);`

------

### **xUnit 的关键特性与关键字**

#### **1.** `[Fact]` **—— 事实**

- **含义**：表示这是一个**确定性**的测试。无论运行多少次，只要代码逻辑不变，结果永远是一样的。
- **适用场景**：测试算法、逻辑判断（如你的天气判断、加减法、字符串处理）。
- **注意**：这就是你之前遇到问题的关键，没有这个标签，xUnit 根本“看不见”你的方法。

#### **2.** `[Theory]` **和** `[InlineData]` **—— 理论**

这是 xUnit 最强大的功能之一。当你想用**多组不同的数据**测试同一个逻辑时，不需要写多个 `[Fact]`，而是用 `[Theory]`。

**优化你的代码示例：**
你写了两个测试方法（35度是热，15度是冷），其实可以合并为一个理论测试：

```
// [Theory] 表示这是一个理论测试，需要数据支持
[Theory]
// [InlineData] 提供具体的数据：输入值, 期望结果
[InlineData(35, "Hot")]
[InlineData(15, "Cold")]
[InlineData(20, "Mild")] // 假设20度是温和
public void GetWeatherDescription_VariousTemps_ReturnsExpected(string expected, int temp)
{
    // Arrange
    var service = new WeatherService();

    // Act
    var result = service.GetWeatherDescription(temp);

    // Assert
    Assert.Equal(expected, result);
}
```

- **效果**：在 Visual Studio 的测试资源管理器中，这一个方法会显示为 3 条测试用例。

#### **3. 构造函数与** `IClassFixture` **—— 生命周期管理**

你之前的代码中使用了构造函数 `public WeatherServiceTests()` 来初始化服务。

- **每个测试方法运行前都会创建一个测试类的新实例**。
- 场景：
  - 如果你的 Service 很轻量（像上面的 WeatherService），在构造函数里 `new` 一个没问题。
  - 如果你的测试需要启动整个数据库、或者构建一个庞大的 `WebApplicationFactory`（集成测试），每次都 `new` 会非常慢。
- **解决方案**：使用 `IClassFixture<T>`。它允许你在多个测试方法之间**共享**同一个上下文（例如共享同一个数据库连接或应用程序实例）。

------

### **常用断言 (Assert) 速查**

xUnit 的 `Assert` 类提供了各种验证方法，这是你编写测试逻辑的核心工具：

| 断言方法           | 说明                             | 示例                                                         |
| :----------------- | :------------------------------- | :----------------------------------------------------------- |
| **Equal**          | 验证两个值相等（最常用）         | `Assert.Equal(4, 2 + 2);`                                    |
| **NotEqual**       | 验证两个值不相等                 | `Assert.NotEqual(5, 2 + 2);`                                 |
| **True / False**   | 验证布尔值                       | `Assert.True(list.Any());`                                   |
| **Null / NotNull** | 验证对象是否为空                 | `Assert.NotNull(user);`                                      |
| **Contains**       | 验证集合或字符串包含某元素       | `Assert.Contains("str", "string");`                          |
| **Throws**         | **验证是否抛出异常**（非常重要） | `Assert.Throws<ArgumentNullException>(() => service.Do(null));` |