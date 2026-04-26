# JsonCpp 库

## Json::Value

`Json::Value` 这个“盒子”非常智能，它可以**根据你放进去的东西，自动变成不同的类型**。它能装：

- 一个 **字符串** (`"hello"`)
- 一个 **数字** (`42`, `3.14`)
- 一个 **布尔值** (`true`, `false`)
- 一个 **空值** (`null`)
- 一组有序的数据（**数组/列表**），比如 `[1, 2, "three"]`
- 一组无序的键值对（**对象/字典**），比如 `{"name": "Alice", "age": 30}`

# Boost 库

## BOOST_ASSERT

`BOOST_ASSERT` 是 **Boost 库**提供的一个**断言 (Assertion) 宏**。你可以把它想象成一个**严格的保安**。

- **它的核心逻辑是**：

  1. **检查**：它会去检查括号 `()` 里的条件（在这里就是 `m_config.isMember("Path")`）。
  2. **通过**：如果条件为 **`true`** (真)，说明一切正常，保安放行，程序继续往下执行。
  3. **失败**：如果条件为 **`false`** (假)，说明出现了**程序员绝不希望发生的情况**（在这里就是“配置文件里居然没有 'Path' 这个必需的字段！”）。这时，保安会立刻拉响警报！

- **“拉响警报”意味着什么**？

  - 程序会**立即终止**。
  - 通常会在**控制台**或**弹出一个错误窗口**，告诉你**在哪一行代码**触发了断言失败，并且会显示失败的**表达式**（即 `m_config.isMember("Path")`）。
  - 这样，开发者就能**第一时间、非常精准地**定位到问题所在。

  ![](D:\Project\Knowledge-Burger\Picture\C++\BOOST ASSERT.png)

## Log

- **`add_common_attributes`**：它会自动向日志系统注册一些**常用的全局属性**
- **`BOOST_LOG_SEV(g_lg, severity)`**
  - **`BOOST_LOG_SEV`**: 这是 **Boost.Log** 库提供的一个**核心宏**，用于记录**带严重级别 (Severity)** 的日志。
  - **`g_lg`**: 这是一个**全局的日志记录器对象**（通常是 `boost::log::sources::severity_logger<...> g_lg;`）。它是你所有日志的“总出口”。
  - **`severity`**: 这就是你传进来的日志级别，比如 `info`, `warning`, `error` 等。
  - **作用**：这行代码会向 `g_lg` 这个记录器发送一条日志，并标记其严重级别为 `severity`。



### attributes

- **`attrs::mutable_constant`**：创建一个可以在运行时被修改的常量属性。

# chrono 库

`<chrono>` 是 C++11 引入的标准库，用于**精确、类型安全地处理时间**。它解决了传统 C 风格时间函数（如 `time_t`、`clock()`）易出错、不直观的问题。

## **📊 常见时间段类型**

| 类型                        | 说明 |
| :-------------------------- | :--- |
| `std::chrono::nanoseconds`  | 纳秒 |
| `std::chrono::microseconds` | 微秒 |
| `std::chrono::milliseconds` | 毫秒 |
| `std::chrono::seconds`      | 秒   |
| `std::chrono::minutes`      | 分钟 |
| `std::chrono::hours`        | 小时 |

# std 库

## std::find_if

- 这是一个**泛型算法**，用于在范围内查找**第一个满足条件**的元素

- 返回指向找到元素的**迭代器**，如果没找到则返回 `end()` 迭代器

  ```
  std::find_if(起始位置, 结束位置, 判断条件);
  ```

- **`m_robots.begin()`**: 容器的起始迭代器

- **`m_robots.end()`**: 容器的结束迭代器（不包含此位置）

- **lambda 表达式**: 判断条件函数

## std::atomic\<T>

**`std::atomic` 用来保证对某个变量的操作是“原子的”——即不会被其他线程打断，从而避免多线程下的数据竞争（data race）和未定义行为。**

## std::mutex

C++ 中用于**自动管理互斥锁（Mutex）** 的经典用法，属于 **RAII（Resource Acquisition Is Initialization）** 编程范式。它的核心目的是：**确保在作用域内安全地加锁，并在离开作用域时自动解锁，防止死锁或资源泄漏**。

```c++
// 事例代码
void some_function() {
    // 进入作用域
    {
        std::lock_guard<std::mutex> locker(m_mutex); // ← 自动加锁！

        // 👇 安全操作共享资源（临界区）
        shared_data++;
        process(shared_data);
        // ...

    } // ← 离开作用域，locker 被销毁 → 自动调用 m_mutex.unlock()！
}
```

## std::shared

`std::make_shared` 是 C++11 引入的一个**工厂函数模板**，用于**安全、高效地创建 `std::shared_ptr` 智能指针**。

```C++
// 基本用法
#include <memory>

// 1. 创建无参构造的对象
auto ptr1 = std::make_shared<MyClass>();

// 2. 创建带参数构造的对象
auto ptr2 = std::make_shared<MyClass>(arg1, arg2, ...);

// 3. 显式指定类型（较少用）
std::shared_ptr<MyClass> ptr3 = std::make_shared<MyClass>("hello", 42);
```

