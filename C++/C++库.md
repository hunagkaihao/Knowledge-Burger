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

## BOOST_ASSERT_MSG

`BOOST_ASSERT_MSG` 是 **Boost 库提供的一个增强版断言宏**，用于在程序运行时检查条件是否成立，并在失败时输出**自定义的错误信息**。

**基本用法**

```C++
#include <boost/assert.hpp>

BOOST_ASSERT_MSG(condition, message);
```

- `condition`：一个布尔表达式（如 `x > 0`）
- `message`：一个 C 字符串（`const char*`），当断言失败时显示

**使用示例**

```c++
#include <boost/assert.hpp>
#include <iostream>

int divide(int a, int b) {
    BOOST_ASSERT_MSG(b != 0, "除数不能为零！"); // ← 关键断言
    return a / b;
}

int main() {
    std::cout << divide(10, 2) << std::endl; // 正常：5
    std::cout << divide(10, 0) << std::endl; // 断言失败！
    return 0;
}
```

## Boost.Filesystem

**boost::filesystem::directory_iterator**：遍历单个目录下所有文件和子目录（不递归）

**boost::filesystem::is_regular_file(entry)**：判断是否为普通文件



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

## std::move

`std::move` 是 C++11 引入的一个**关键工具**，用于启用 **移动语义（Move Semantics）**，从而**避免不必要的深拷贝、提升性能**。但它本身**并不执行移动操作**，而是一个“类型转换”工具。

**`std::move` 的作用是：告诉编译器——“这个东西我不用了，你可以直接‘拿走’它的内容，不用复制！”**

```C++
#include <iostream>
#include <vector>

int main() {
    std::vector<int> boxA = {1, 2, 3, 4, 5}; // 装满数字的“箱子”
    
    // 方式1：拷贝（慢，占内存）
    std::vector<int> boxB = boxA; // 复制所有数字 → 花时间！

    // 方式2：移动（快！）
    std::vector<int> boxC = std::move(boxA); // 直接“拿走”boxA的内容

    // 此时：
    // - boxC 有 {1,2,3,4,5}
    // - boxA 变成空的（不能再用！）
}
```

## std::bind

`std::bind` 是 C++11 引入的一个强大工具，用于**将函数（或可调用对象）与其参数绑定**，生成一个新的可调用对象。它常用于：

- 固定部分参数（实现“柯里化”）
- 调整参数顺序
- 将成员函数转换为普通函数形式
- 适配回调接口（如线程、算法）
  

**基础语法**

```c++
#include <functional> // 必须包含
using namespace std::placeholders; // 用于 _1, _2, ...

auto new_func = std::bind(原函数, 参数1, 参数2, ..., _1, _2, ...);
```

- `_1`, `_2`, ... 是**占位符**，表示新函数的第1、第2个参数。
- 绑定后的对象可以像函数一样调用：`new_func(arg1, arg2);`

**使用示例**

```c++
#include <iostream>
#include <functional>

void print(int a, int b, int c) {
    std::cout << a << ", " << b << ", " << c << "\n";
}

int main() {
    using namespace std::placeholders;

    // 固定第一个参数为 10，第二个参数由调用者提供，第三个固定为 30
    auto f = std::bind(print, 10, _1, 30);

    f(20); // 输出: 10, 20, 30
}
```

## std::map

`std::map` 是标准模板库（STL）中的一种**关联容器（Associative Container）**，用于存储 **键值对（key-value pairs）**，其中每个键（key）都是唯一的，并自动按键进行**排序**。

std::map<Key, Value> 中的每个元素实际上是一个 std::pair<const Key, Value> 类型的对象。

- .first → 键（key）
- .second → 值（value）

1. **基本特性**

| 特性             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| 头文件           | `#include <map>`                                             |
| 底层实现         | 通常为 红黑树（Red-Black Tree）（C++ 标准未强制，但主流编译器如此） |
| 是否有序         | ✅ 按键自动升序排序（可自定义比较函数）                       |
| 键是否唯一       | ✅ 是（若需重复键，用 `std::multimap`）                       |
| 时间复杂度       | 插入/查找/删除：O(log n)                                     |
| 是否支持随机访问 | ❌ 不支持（不能用 `map[5]` 表示第5个元素）                    |

2. **基本声明与初始化**

```c++
#include <iostream>
#include <map>
#include <string>

// 声明：map<键类型, 值类型>
std::map<int, std::string> id_to_name;
std::map<std::string, double> price_list;
```

**初始化方法**

```c++
// 1. 空 map
std::map<std::string, int> scores;

// 2. 初始化列表（C++11 起）
std::map<std::string, int> scores = {
    {"Alice", 95},
    {"Bob", 87},
    {"Charlie", 92}
};

// 3. 复制构造
std::map<std::string, int> copy_scores(scores);
```

3. **常用操作**

```C++
// 插入元素
// 方法1：使用 [] 操作符（会覆盖已存在的键）
scores["David"] = 88;        // 若 "David" 不存在则插入；存在则修改
// 方法2：insert() —— 不会覆盖已有键
scores.insert({"Eve", 90});
scores.insert(std::make_pair("Frank", 85));
// 方法3：emplace()（推荐，避免临时对象）
scores.emplace("Grace", 93);

// 访问元素
// 1. 使用 []（可读可写，但会插入默认值如果不存在！）
int a = scores["Alice"];     // 安全
int b = scores["Unknown"];   // 会插入 {"Unknown", 0}！
// 2. 使用 at()（安全，不存在时抛出 std::out_of_range）
try {
    int c = scores.at("Bob");
} catch (const std::out_of_range& e) {
    std::cout << "Key not found!\n";
}
// 3. 使用 find()（推荐用于检查存在性）
auto it = scores.find("Charlie");
if (it != scores.end()) {
    std::cout << "Found: " << it->second << "\n";
}

// 遍历 map
// C++11 范围 for 循环（推荐）
for (const auto& pair : scores) {
    std::cout << pair.first << ": " << pair.second << "\n";
}
// 或使用迭代器
for (auto it = scores.begin(); it != scores.end(); ++it) {
    std::cout << it->first << " => " << it->second << "\n";
}

// 删除元素
// C++11 范围 for 循环（推荐）
for (const auto& pair : scores) {
    std::cout << pair.first << ": " << pair.second << "\n";
}
// 或使用迭代器
for (auto it = scores.begin(); it != scores.end(); ++it) {
    std::cout << it->first << " => " << it->second << "\n";
}

// 查询信息
size_t n = scores.size();        // 元素个数
bool empty = scores.empty();     // 是否为空
// 查找是否存在
if (scores.count("Charlie") > 0) {
    // 存在（count 返回 0 或 1）
}
```
