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

# DisplayFormat

`DisplayFormat` 特性（`[DisplayFormat]`）是 .NET (ASP.NET MVC, ASP.NET Core, Blazor) 中用于**控制数据在 UI 层显示格式**的一个强大工具。它属于 `System.ComponentModel.DataAnnotations` 命名空间。

它的核心作用是：**将“数据逻辑”与“显示逻辑”分离**。你不需要在 View 或 Razor 页面中写复杂的格式化字符串（如 `{0:C}`, `{0:yyyy-MM-dd}`），而是直接在 Model 属性上声明。

###  **1. 基本用法**

首先需要引入命名空间：

```C#
using System.ComponentModel.DataAnnotations;
```

#### **核心属性**

- **`DataFormatString`**: 格式化字符串（如 `"{0:C}"`, `"{0:yyyy-MM-dd}"`）。
- **`ApplyFormatInEditMode`**: (关键) 是否在编辑模式（如 `<input>` 框）下也应用此格式。默认为 `false`。

### **2. 常见场景案例**

#### **场景 A：货币与数字格式化**

```C#
public class Product
{
    public int Id { get; set; }

    [Display(Name = "产品名称")]
    public string Name { get; set; }

    // 格式化为货币 (自动带符号和两位小数，如 ¥1,234.56)
    [DisplayFormat(DataFormatString = "{0:C}", ApplyFormatInEditMode = false)]
    public decimal Price { get; set; }

    // 格式化为百分比 (如 0.15 -> 15.00%)
    [DisplayFormat(DataFormatString = "{0:P2}", ApplyFormatInEditMode = false)]
    public double DiscountRate { get; set; }

    // 保留两位小数，千分位分隔
    [DisplayFormat(DataFormatString = "{0:N2}", ApplyFormatInEditMode = false)]
    public double Weight { get; set; }
}
```

> **注意**：`{0:C}` 会根据当前线程的 `CultureInfo` 自动适配货币符号（人民币¥, 美元 $ , 欧元€等）。

#### **场景 B：日期与时间格式化**

这是最常用的场景，避免显示 `2023/10/5 14:30:00` 这种包含毫秒的冗长格式。

```C#
public class Event
{
    // 只显示日期：2023-10-05
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = false)]
    public DateTime EventDate { get; set; }

    // 显示日期和时间：2023-10-05 14:30
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd HH:mm}", ApplyFormatInEditMode = false)]
    public DateTime StartDateTime { get; set; }
    
    // 短日期格式 (根据系统区域设置，如 10/5/2023)
    [DisplayFormat(DataFormatString = "{0:d}", ApplyFormatInEditMode = false)]
    public DateTime ShortDate { get; set; }
}
```

#### **场景 C：空值处理 (Null Display)**

当数据库值为 `null` 时，默认显示为空字符串。你可以自定义显示内容（如 "暂无"、"N/A"）。

```C#
public class User
{
    // 如果 MiddleName 为 null，界面显示 "未填写"
    [DisplayFormat(NullDisplayText = "未填写")]
    public string MiddleName { get; set; }

    // 如果 LastLoginTime 为 null，显示 "从未登录"
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", NullDisplayText = "从未登录")]
    public DateTime? LastLoginTime { get; set; }
}
```

**常用格式化字符串速查表**

| 占位符 | 示例代码      | 输出示例 (zh-CN) | 说明                             |
| ------ | ------------- | ---------------- | -------------------------------- |
| C      | `{0:C}`       | ¥1,234.56        | 货币 (Currency)                  |
| D      | `{0:D5}`      | 00123            | 十进制整数 (Decimal)，补零       |
| F      | `{0:F2}`      | 123.45           | 定点数 (Fixed-point)，指定小数位 |
| N      | `{0:N0}`      | 1,235            | 数字 (Number)，带千分位          |
| P      | `{0:P1}`      | 12.3%            | 百分比 (Percent)                 |
| d      | `{0:d}`       | 2023/10/5        | 短日期                           |
| D      | `{0:D}`       | 2023年10月5日    | 长日期                           |
| t      | `{0:t}`       | 14:30            | 短时间                           |
| T      | `{0:T}`       | 14:30:05         | 长时间                           |
| 自定义 | `{0:yyyy-MM}` | 2023-10          | 自定义日期格式                   |

# DatabaseGenerated

`[DatabaseGenerated]` 是 Entity Framework (EF) 和 EF Core 中用于**明确指定数据库如何生成列值**的数据注解（Data Annotation）。它位于 `System.ComponentModel.DataAnnotations.Schema` 命名空间下。

它的核心作用是告诉 EF：**“这个属性的值不是我代码里赋的，而是数据库自己生成的（或者由数据库触发器/默认值处理），请不要在 INSERT/UPDATE 语句中包含它，或者在操作后去数据库把新值取回来。”**

### **1. 三种枚举值详解**

该特性接受一个 `DatabaseGeneratedOption` 枚举参数，共有三种模式：

#### **A.** `DatabaseGeneratedOption.Identity` **(默认/自增)**

- **含义**：值由数据库在**插入时**生成（通常是自增主键 ID、SQL Server 的 `IDENTITY`、PostgreSQL 的 `SERIAL`）。
- 行为：
  - EF 在 `INSERT` 语句中**不包含**此列。
  - EF 在插入后立即执行查询，获取数据库生成的新值并更新到实体对象中。
  - **无法**手动修改此值（如果强行赋值，EF 通常会忽略或报错，取决于配置）。
- **适用场景**：主键 `Id`、自增序列号。

#### **B.** `DatabaseGeneratedOption.Computed` **(计算列/始终生成)**

- **含义**：值由数据库在**插入和更新时**都生成（例如：时间戳、计算列、由触发器生成的值）。
- 行为：
  - EF 在 `INSERT` 和 `UPDATE` 语句中都**不包含**此列。
  - EF 每次保存后都会从数据库读取最新值。
  - **代码中对该属性的赋值会被完全忽略**。
- **适用场景**：`RowVersion` (并发控制)、`CreatedAt` (默认值)、计算列 (如 `TotalPrice = Qty * Price`)。

#### **C.** `DatabaseGeneratedOption.None` **(无/手动管理)**

- **含义**：值**不**由数据库生成，完全由应用程序代码管理。
- 行为：
  - EF 会在 `INSERT` 和 `UPDATE` 语句中**包含**此列，并使用你赋予的值。
  - 如果你不赋值，可能会报“非空列不能为 null”的错误（除非数据库有默认值且 EF 配置允许）。
- **适用场景**：GUID 主键 (`Guid.NewGuid()` 在代码中生成)、自然主键（如身份证号、订单号）。

# UnitOfWork

`[UnitOfWork]` 是 **ABP 框架**（ASP.NET Boilerplate / ABP VNext）中的一个**声明式事务管理特性**，它的核心作用是：**将标记的方法内的所有数据库操作封装在同一个事务中，确保"要么全部成功提交，要么全部失败回滚"**。

### **工作原理**

ABP 框架底层使用 **Castle DynamicProxy（动态代理）** 技术来拦截被标记的方法调用。当方法执行时，框架会自动完成以下流程：

1. **方法执行前**：自动开启一个数据库事务，并创建数据库上下文（DbContext）
2. **方法执行中**：所有数据库操作共享同一个上下文和事务
3. **方法正常结束**：自动提交事务（Commit）
4. **方法抛出异常**：自动回滚事务（Rollback）

### **代码示例**

假设你的方法被 `[UnitOfWork]` 标记：

```
[UnitOfWork]
public virtual async Task UpdateAndDeleteAsync()
{
    // 第一步：修改某个字段 → 执行成功 ✅
    var entity = await _repo.GetAsync(1);
    entity.Name = "新名称";
    await _repo.UpdateAsync(entity);

    // 第二步：删除某个字段 → 执行失败 ❌（比如外键约束、数据不存在等）
    await _repo.DeleteAsync(999); // 假设这里抛异常了
}
```

**执行结果**：

- 第一步的修改操作虽然已经执行了，但因为第二步抛出了异常，事务会自动**回滚（Rollback）**
- 第一步的修改也会被**撤销**，数据库恢复到方法执行前的状态
- 就好像这两步操作**从来没有发生过**一样

 **如果没有事务保护会怎样？**

```
// ❌ 没有 [UnitOfWork]，没有事务保护
public async Task UpdateAndDeleteAsync()
{
    // 第一步：修改成功，数据已经写入数据库了
    var entity = await _repo.GetAsync(1);
    entity.Name = "新名称";
    await _repo.UpdateAsync(entity); // 数据已落库

    // 第二步：删除失败，抛异常
    await _repo.DeleteAsync(999); // 💥 异常！

    // 结果：修改已经生效了，但删除没执行
    // 数据库处于"半完成"的不一致状态！
}
```