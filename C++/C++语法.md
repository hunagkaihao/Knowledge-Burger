# atoi

`atoi` 是 C/C++ 标准库中的一个函数，用于将 **C 风格字符串（`const char\*`）转换为整数（`int`）**。

- **函数原型**

  ```C++
  #include <cstdlib>  // C++ 中推荐包含此头文件
  // 或 #include <stdlib.h>  // C 风格
  
  int atoi(const char* str);
  ```

- **功能说明**

  - 将字符串 `str` 开头的 **十进制数字** 转换为 `int` 类型。
  - 会自动跳过开头的 **空白字符**（如空格、制表符 `\t`、换行 `\n` 等）。
  - 遇到第一个 **非数字字符**（除正负号外）时停止转换。
  - 如果字符串 **无法转换**（如全为字母），返回 `0`。
  - **不报告错误**：无法区分“转换结果为 0”和“转换失败”。

- **使用示例**

  ```C++
  #include <iostream>
  #include <cstdlib>
  using namespace std;
  
  int main() {
      cout << atoi("123")        << endl;  // 输出: 123
      cout << atoi("-456")       << endl;  // 输出: -456
      cout << atoi("  789 ")     << endl;  // 输出: 789（忽略前后空格）
      cout << atoi("123abc")     << endl;  // 输出: 123（遇到 'a' 停止）
      cout << atoi("abc123")     << endl;  // 输出: 0（开头非数字）
      cout << atoi("")           << endl;  // 输出: 0（空字符串）
      cout << atoi("2147483648") << endl;  // 可能溢出！结果未定义（通常是 -2147483648）
      return 0;
  }
  ```

# modbus_write_bit

`modbus_write_bit` 是 **libmodbus** 库中的一个函数，用于向 Modbus 从站（Slave）**写入单个线圈（Coil）的状态**（ON/OFF，即 1/0）。

- **函数原型**

  ```C++
  #include <modbus.h>
  
  int modbus_write_bit(modbus_t *ctx, int addr, int status);

- **参数说明**

  | 参数     | 类型        | 说明                                                         |
  | -------- | ----------- | ------------------------------------------------------------ |
  | `ctx`    | `modbus_t*` | Modbus 上下文指针（由 `modbus_new_tcp()`、`modbus_new_rtu()` 等创建） |
  | `addr`   | `int`       | 线圈地址（Coil Address），通常从 `0` 开始（对应 Modbus 协议中的 0xxxx 寄存器，如 00001） |
  | `status` | `int`       | 要写入的值：  - `0` 表示 OFF（断开）  - 非 0（通常用 `1`）表示 ON（闭合） |

📌 注意：Modbus 协议中线圈地址是 **1-based**（如设备文档写“线圈 00001”），但 **libmodbus 使用 0-based 地址**。
所以：设备文档说 “写线圈 00001” → 代码中 `addr = 0`；写线圈 00100 → `addr = 99`

- **返回值**

  **成功**：返回 `1`（表示成功写入 1 个线圈）

  **失败**：返回 `-1`，并可通过 `modbus_strerror(errno)` 获取错误信息

- **使用示例**

  **示例 1：TCP 模式写线圈**

  ```C++
  #include <modbus.h>
  #include <stdio.h>
  #include <errno.h>
  
  int main() {
      modbus_t *ctx;
      ctx = modbus_new_tcp("192.168.1.10", 502); // 连接从站 IP 和端口
      if (ctx == NULL) {
          fprintf(stderr, "无法创建 Modbus TCP 上下文\n");
          return -1;
      }
  
      // 设置从站 ID（默认是 1，可省略）
      modbus_set_slave(ctx, 1);
  
      // 写线圈 00001（地址 0）为 ON（1）
      int rc = modbus_write_bit(ctx, 0, 1);
      if (rc == 1) {
          printf("写线圈成功！\n");
      } else {
          fprintf(stderr, "写线圈失败: %s\n", modbus_strerror(errno));
      }
  
      modbus_close(ctx);
      modbus_free(ctx);
      return 0;
  }

​		**示例 2 ：RTU 模式（串口）**

```C++
modbus_t *ctx = modbus_new_rtu("/dev/ttyUSB0", 9600, 'N', 8, 1);
modbus_set_slave(ctx, 1);
modbus_connect(ctx);

// 写线圈地址 5（即设备文档中的 00006）为 OFF
modbus_write_bit(ctx, 5, 0);

modbus_close(ctx);
modbus_free(ctx);
```

# modbus_strerror

`modbus_strerror` 是 **libmodbus** 库提供的一个函数，用于将 **错误码（通常是 `errno`）转换为人类可读的错误描述字符串**，便于调试和日志记录。

- **函数原型**

  ```C++
  #include <modbus.h>
  
  const char *modbus_strerror(int errnum);
  ```

- **功能说明**

  - 将传入的错误码 `errnum` 转换为对应的错误信息字符串。
  - 它既支持 **标准系统错误码**（如 `ECONNREFUSED`, `ETIMEDOUT`），也支持 **libmodbus 特有的错误码**（如 `MB_ENOBASE`, `MB_EIO` 等）。
  - 返回的是 **静态字符串指针**，无需手动释放。

  > ✅ 通常与 `errno` 配合使用，在 libmodbus 函数（如 `modbus_connect`, `modbus_read_registers`）返回 `-1` 后调用。

- **使用示例**

  **示例 1：连接失败时打印错误**

  ```C++
  modbus_t *ctx = modbus_new_tcp("192.168.1.100", 502);
  if (modbus_connect(ctx) == -1) {
      fprintf(stderr, "连接失败: %s\n", modbus_strerror(errno));
      modbus_free(ctx);
      return -1;
  }
  ```

  **示例 2：写线圈超时时打印详细信息**

  ```C++
  int rc = modbus_write_bit(ctx, 0, 1);
  if (rc == -1) {
      CTF_LOG(LOG_ERROR, "写线圈失败: %s", modbus_strerror(errno));
  }
  ```

- **常见错误码及含义（结合 `modbus_strerror`)

  | 错误码（`errno` 值） | 错误字符串（`modbus_strerror` 返回） | 原因                                  |
  | -------------------- | ------------------------------------ | ------------------------------------- |
  | `ETIMEDOUT` (110)    | `"timed out"`                        | 等待从站响应超时（最常见！）          |
  | `ECONNREFUSED` (111) | `"Connection refused"`               | TCP 连接被拒绝（IP/端口错、服务未开） |
  | `EHOSTUNREACH` (113) | `"No route to host"`                 | 网络不可达                            |
  | `EINVAL` (22)        | `"Invalid argument"`                 | 地址或参数非法                        |
  | `EIO` (5)            | `"Input/output error"`               | 串口通信错误（RTU 模式常见）          |
  | `MB_ENOBASE` (-1)    | `"Invalid Modbus ID"`                | 从站地址无效（libmodbus 特有）        |
  | `MB_EILLFUNC` (-2)   | `"Illegal function"`                 | 从站不支持该功能码                    |

 # ::

在 C++ 中，`::` 被称为 **作用域解析运算符（Scope Resolution Operator）**。它是 C++ 语言中非常重要的一个符号，用于指定某个标识符（如变量、函数、类等）所属的作用域。

1. **基本语法**

   ```
   作用域名::标识符

- ```
  作用域名
  ```

   可以是：

  - 全局命名空间（用空表示）
  - 命名空间（namespace）
  - 类（class/struct）

- `标识符` 可以是变量、函数、类型、枚举值等。

2. **主要用途**

   (1) **访问全局作用域中的变量或函数**

   ```C++
   #include <iostream>
   int x = 10; // 全局变量
   
   int main() {
       int x = 20; // 局部变量
       std::cout << x << std::endl;        // 输出 20（局部）
       std::cout << ::x << std::endl;      // 输出 10（全局）
       return 0;
   }
   
   (2) **定义类的成员函数（在类外）**

``` C++
class MyClass {
public:
    void func(); // 声明
};

// 在类外定义：使用 :: 指明 func 属于 MyClass
void MyClass::func() {
    std::cout << "Hello from MyClass::func";
}
```

   (3) **访问命名空间中的内容**

```C++
namespace MyLib {
    int value = 42;
    void print() { std::cout << "MyLib::print"; }
}

int main() {
    std::cout << MyLib::value << std::endl;   // 42
    MyLib::print();                           // 调用命名空间中的函数
    return 0;
}
```

# map

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

# static

控制生命周期与作用域：局部静态、类静态、文件静态



# 堆 VS 栈

| 特性     | 栈（Stack）                  | 堆（Heap）                                   |
| -------- | ---------------------------- | -------------------------------------------- |
| 分配方式 | 编译器自动分配/释放          | 程序员手动 `new`/`delete` 或 `malloc`/`free` |
| 生命周期 | 作用域结束自动销毁           | 手动释放，否则内存泄漏                       |
| 速度     | 极快（只需移动栈指针）       | 较慢（需查找空闲内存块）                     |
| 大小限制 | 通常较小（几 MB）            | 较大（受虚拟内存限制）                       |
| 碎片     | 无碎片                       | 可能产生内存碎片                             |
| 存储内容 | 局部变量、函数参数、返回地址 | 动态分配的对象、大数组、跨作用域数据         |
| 安全性   | 自动管理，不易出错           | 易出错（野指针、重复释放、泄漏）             |

# 虚函数

虚函数是基类中的成员函数，可以在派生类中被重写。

虚函数是 C++ 中多态性的关键组成部分。它们允许不同的对象对同一个函数调用做出不同的响应。



**实现原理：虚函数表（vtable）**

- 每个含虚函数的类有一个 **虚函数表（vtable）**
- 每个对象包含一个 **虚表指针（vptr）**，指向其类的 vtable
- 调用虚函数时，通过 `vptr → vtable → 函数地址` 动态分发



# 智能指针

`概念`：C++11 引入智能指针，自动管理堆内存，防止泄漏。

# 标准库类型 Vector

`概念`：标准库类型 vector 表示对象的集合，其中所有对象的类型都相同。

集合中的每个对象都有一个与之对应的索引，索引用于访问对象。

因为 vector “容纳着” 其他对象，所以它也常被称作**容器**。

| 方法                         | 含义                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| vector\<T> v1                | v1 是一个空 vector， 它潜在的元素是 T 类型的。执行默认初始化 |
| vector\<T> v2 (v1)           | v2 中包含有 v1 所有元素的副本                                |
| vector\<T> v2 = v1           | 等价于 v2 (v1)，v2 中包含有 v1 所有元素的副本                |
| vector\<T> v3 (n, val)       | v3 包含了 n 个重复元素，每个元素的值都是 val                 |
| vector\<T> v4 (n)            | v4 包含了 n 个重复地执行了初始化的对象                       |
| vector\<T> v5 {a, b, c...}   | v5 包含了初始值个数的元素，每个元素被赋予相应的初始值        |
| vector\<T> v5 = {a, b, c...} | 等价于 v5 (a, b, c...)                                       |

注意：

1. 如果用的是**圆括号**，可以说提供的值是用来**构造**（construct）vector 对象的。
2. 如果用的是**花括号**，可以表述成我们想**列表初始化**（list initialize）该 vector 对象。
3. 要想列表初始化 vector 对象，花括号里的值必须与元素**类型相同**。确定无法执行列表初始化后，编译器会尝试用默认值初始化 vector 对象。

## vector 支持的操作

| 方法              | 含义                                                         |
| ----------------- | ------------------------------------------------------------ |
| v.empty()         | 如果 v 不含有任何元素，返回真；否则返回假                    |
| v.size()          | 返回 v 中元素的个数                                          |
| v.push_back(t)    | 向 v 的尾端添加一个值为 t 的元素                             |
| v.emplace_back(t) | 向 v 的尾端添加一个值为 t 的元素，直接在容器内部“原地构造”对象 |
| v[n]              | 返回 v 中第 n 个位置上元素的索引                             |
| v1 = v2           | 用 v2 中元素的拷贝替换 v1 中的元素                           |
| v1 = {a, b, c...} | 用列表中元素的拷贝替换 v1 中的元素                           |

# 空语句

最简单的语句是**空语句**（null statement），空语句中只会含有一个单独的分号：

```C++
;	// 空语句
```

## 使用场景

如果在程序的某个地方，语法上需要一条语句但是逻辑上不需要，此时应该使用空语句。

一种常见的情况是，当循环的全部工作在条件部分就可以完成时，我们通常会用到空语句。

```C++
// 重复读入数据直至到达文件末尾或某次输入的值等于 sought
while (cin >> s && s != sought)
    ; // 空语句
```

# ->

间接成员访问

当你有一个**指针**或**行为像指针的对象**（如迭代器、智能指针）时。

# .

直接成员访问

当你有一个**对象实例**本身时。

# typedef

C++ 中的“类型重命名”关键字。它不创建新类型，只是给现有类型起个**更短、更易读的别名**。

```c++
typedef std::map<int32_t, MapImpPtr> Maps;

// 现在你可以这样声明变量：
Maps all_maps;  // 等价于 std::map<int32_t, MapImpPtr> all_maps;
```

# isMember(key)

**用来检查一个 JSON 对象中是否包含某个字段（key）**
