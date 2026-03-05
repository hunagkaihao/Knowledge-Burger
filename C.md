# **`回调函数（Callback Function）`**

是一种通过函数指针传递给另一个函数，并在特定时机被调用的函数

1. **函数指针声明**

   ```C
   // 声明一个返回 int、接受两个 int 参数的函数指针类型
   typedef int (*CompareFunc)(int a, int b);
   ```

2. **回调函数所定义**

   ```C
   int compare(int a, int b) {
       return a - b; // 升序
   }
   ```

3. **接受回调函数的函数**

   ```C
   void my_sort(int arr[], int n, CompareFunc cmp) {
       // 简单冒泡排序，使用 cmp 回调比较元素
       for (int i = 0; i < n - 1; i++) {
           for (int j = 0; j < n - i - 1; j++) {
               if (cmp(arr[j], arr[j + 1]) > 0) {
                   int temp = arr[j];
                   arr[j] = arr[j + 1];
                   arr[j + 1] = temp;
               }
           }
       }
   }
   ```

4. **C调用示例**

   ```C
   #include <stdio.h>
   
   int main() {
       int arr[] = {5, 2, 9, 1, 5, 6};
       int n = sizeof(arr) / sizeof(arr[0]);
   
       my_sort(arr, n, compare);
   
       for (int i = 0; i < n; i++) {
           printf("%d ", arr[i]);
       }
       // 输出：1 2 5 5 6 9
       return 0;
   }	
   ```


___
___
___
# **`memset`**

是 C 语言标准库 `<string.h>` 中的一个常用函数，用于**将一块内存区域的每个字节设置为指定的值**。它常用于初始化数组、结构体或清零内存

一、**函数调用**

```C
#include <string.h>
void *memset(void *s, int c, size_t n);
```

**参数说明**

- `void *s`：指向要操作的内存块的起始地址（可以是数组、结构体、指针等）
- `int c`：要写入的值（注意：以 **字节** 为单位，实际只取其低 8 位，即 `unsigned char` 类型）
- `size_t n`：要设置的字节数

**返回值**

- 返回 `s` 的原始指针（即传入的第一个参数），便于链式调用（虽然很少用）

二、**基本用法示例**

1. **清零数组**

   ```C
   #include <stdio.h>
   #include <string.h>
   
   int main() {
       int arr[5] = {1, 2, 3, 4, 5};
       memset(arr, 0, sizeof(arr)); // 将整个数组清零
   
       for (int i = 0; i < 5; i++) {
           printf("%d ", arr[i]); // 输出：0 0 0 0 0
       }
       return 0;
   }
   ```

2. **初始化字符数组**

   ```c
   char str[10];
   memset(str, 'A', sizeof(str)); // 全部设为 'A'
   // 注意：这不是字符串（没有 '\0' 结尾）
   ```

3. **清空结构体**

   ```C
   struct Point {
       int x, y;
       double z;
   };
   
   struct Point p = {1, 2, 3.14};
   memset(&p, 0, sizeof(p)); // 所有成员归零
   ```
    **⚠️**注意：**不能用于非字节可设值的类型（尤其是非零值初始化整型/浮点型）**

___
___
___

# **`strcpy（string copy）`**

是 C 标准库 `<string.h>` 中的字符串拷贝函数，作用是**将一个字符串（包括末尾的 `\0` 结束符）完整拷贝到另一个字符数组中**

一、**函数调用**

```C
char *strcpy(char *dest, const char *src);
```

**参数说明**

`dest`：目标字符数组（接收拷贝内容的内存空间）；

`src`：源字符串（被拷贝的字符串，`const` 表示不会修改源字符串）；

返回值：返回 `dest` 的首地址（方便链式调用）。

二、**基本用法示例**

```C
#include <stdio.h>
#include <string.h>  // 必须包含此头文件

int main() {
    // 1. 定义目标数组（需确保空间足够）
    char dest[20];
    // 2. 定义源字符串
    const char *src = "Hello World";
    
    // 3. 执行拷贝
    strcpy(dest, src);
    
    // 4. 输出结果
    printf("拷贝后的字符串：%s\n", dest);  // 输出：拷贝后的字符串：Hello World
    return 0;
}
```

三、**注意事项**

1. **必须保证 dest 空间足够大**：如果 `src` 的长度超过 `dest` 的容量，会发生**缓冲区溢出**，破坏内存中其他数据，导致程序崩溃或安全漏洞。
2. **dest 必须是可修改的内存（不能是字符串常量）**：字符串常量（如 `char *dest = "abc";`）存储在只读内存区，用 `strcpy` 向其拷贝会触发内存写入错误
3. **src 必须以 \0 结尾**：如果 `src` 是没有 `\0` 的字符数组，`strcpy` 会一直往后拷贝，直到找到内存中的 `\0`，导致越界
4. **安全替代方案**：为了避免缓冲区溢出，推荐使用更安全的 `strncpy`（指定最大拷贝长度）

___
___
___

# **`memcpy（memory copy）`**

是 C 标准库 `<string.h>` 中的**内存拷贝函数**，作用是**从指定源内存地址拷贝指定字节数的数据到目标内存地址**。它不关心拷贝的数据类型，只按 “字节” 操作，是比 `strcpy` 更通用、更底层的内存操作函数。

一、**函数调用**

```C
void *memcpy(void *dest, const void *src, size_t n);
```

**参数说明**


- `dest`：目标内存地址（接收拷贝数据的起始位置）；

- `src`：源内存地址（被拷贝数据的起始位置）；

- `n`：要拷贝的**字节数**（`size_t` 是无符号整数类型）；

**返回值**

- 返回 `dest` 的首地址（方便链式调用）。

二、**基本用法示例**

1. **拷贝字符串**

```C
#include <stdio.h>
#include <string.h>

int main() {
    char dest[20];
    const char *src = "Hello memcpy";
    
    // 拷贝整个字符串（含\0）：先算长度+1（\0），再拷贝对应字节
    memcpy(dest, src, strlen(src) + 1);
    printf("拷贝字符串：%s\n", dest);  // 输出：拷贝字符串：Hello memcpy
    return 0;
}
```

2. **拷贝整形数组**

```C
#include <stdio.h>
#include <string.h>

int main() {
    int src[] = {1, 2, 3, 4, 5};
    int dest[5];
    
    // 拷贝5个int类型数据：总字节数 = 元素个数 * 单个元素大小
    memcpy(dest, src, sizeof(src));
    
    // 输出目标数组
    for (int i = 0; i < 5; i++) {
        printf("%d ", dest[i]);  // 输出：1 2 3 4 5
    }
    return 0;
}
```

3. **拷贝结构体**

```C
#include <stdio.h>
#include <string.h>

// 定义结构体
typedef struct {
    char name[20];
    int age;
} Person;

int main() {
    Person src = {"张三", 20};
    Person dest;
    
    // 拷贝整个结构体（按字节拷贝）
    memcpy(&dest, &src, sizeof(Person));
    
    printf("姓名：%s，年龄：%d\n", dest.name, dest.age);  // 输出：姓名：张三，年龄：20
    return 0;
}
```

三、**注意事项**

1. **必须保证目标内存空间足够**
2. **避免源和目标内存重叠**
3. **n 的取值要精准**

___
___
___

# **`free`**

是 C 语言标准库 `<stdlib.h>` 中用于**释放动态分配的内存**的函数。它是与 `malloc`、`calloc`、`realloc` 配对使用的，用于将堆（heap）上申请的内存归还给系统，防止**内存泄漏**

一、**函数调用**

```C
#include <stdlib.h>
void free(void *ptr);
```

**参数说明**

- void *ptr：指向之前通过 `malloc`/`calloc`/`realloc` 分配的内存块的指针。
- 如果 `ptr` 是 `NULL`，`free` **不做任何操作**（安全）。
- 如果 `ptr` 不是有效堆地址（如栈变量、已释放的指针、非法地址），行为**未定义**（可能导致程序崩溃或安全漏洞）。

**返回值**

- 无 （void)

二、**基本用法示例**

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    // 动态分配 100 字节
    char *buffer = (char *)malloc(100);
    
    if (buffer == NULL) {
        fprintf(stderr, "内存分配失败！\n");
        return 1;
    }

    strcpy(buffer, "Hello, dynamic memory!");
    printf("%s\n", buffer);

    // 使用完毕，释放内存
    free(buffer);

    // 建议：将指针置为 NULL，避免“悬空指针”
    buffer = NULL;

    return 0;
}
```

三、**注意事项**

1. **只能释放堆内存**
2. **不能重复释放**
3. **不能释放部份内存地址**
4. **free(NULL)是安全的**
5. **释放后不要继续使用（悬空指针）**

___

___

___

# `#ifdef` 和 `#endif` 

是 C/C++ 中的 **预处理指令（Preprocessor Directives）**，用于实现 **条件编译（Conditional Compilation）** —— 即根据某些条件决定是否编译某段代码

一、**基本语法**

```C
#ifdef 宏名
    // 如果“宏名”被定义了（#define），则编译这里的代码
#endif

// 或配合 #else 使用

#ifdef 宏名
    // 宏已定义时编译
#else
    // 宏未定义时编译
#endif
```

二、**基本用法示例**

1. **调试开关**

```C
#define DEBUG_MODE

int main() {
#ifdef DEBUG_MODE
    printf("Debug: entering function...\n");
    printf("Value of x = %d\n", x);
#endif

    // 正常业务逻辑
    do_something();
    return 0;
}
```

2. **平台/芯片差异适配**

```C
#ifdef STM32F407
    RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;
#elif defined(STM32H743)
    RCC->AHB4ENR |= RCC_AHB4ENR_GPIOAEN;
#else
    #error "Unsupported MCU"
#endif
```

3. **功能模块开关**

```C
// config.h
#define USE_WIFI_MODULE
// #define USE_BLE_MODULE  // 暂时不启用 BLE

// main.c
#ifdef USE_WIFI_MODULE
    wifi_init();
    wifi_connect("mySSID", "password");
#endif

#ifdef USE_BLE_MODULE
    ble_init();
#endif
```

4. **防止头文件重复包含（传统方式**

```C
// my_header.h
#ifndef MY_HEADER_H
#define MY_HEADER_H

// 头文件内容
int global_var;
void my_function(void);

#endif // MY_HEADER_H
```

三、**注意事项**

1. #ifdef 只看 "是否定义"， 不看 ”值是什么“

---

---

---

#  **`osThreadNew` **
是 **CMSIS-RTOS v2**（ARM 定义的 RTOS 标准 API）中用于**创建并启动一个新任务（线程）** 的核心函数。它替代了 CMSIS-RTOS v1 中的 `osThreadCreate`，采用更现代、类型安全、可配置的方式。

一、**函数原型**

```C
osThreadId_t osThreadNew (
    osThreadFunc_t func,        // 任务入口函数（必须）
    void *argument,             // 传递给任务的参数（可为 NULL）
    const osThreadAttr_t *attr  // 任务属性（可为 NULL，使用默认值）
);
```

**返回值**：

- 成功：返回 任务 ID（`osThreadId_t` 类型，非 NULL）
- 失败：返回 NULL

二、**参数详解**

1. `osThreadFunc_t func`

   ```C
   typedef void (*osThreadFunc_t)(void *argument);
   // 必须是一个**永不返回**的函数（通常包含 `for(;;)` 或 `while(1)` 循环）
   // 示例
   void MyTask(void *arg) {
       uint32_t id = *(uint32_t*)arg;
       for (;;) {
           printf("Task %lu running\n", id);
           osDelay(1000);
       }
   }
   ```
   
2. `void *argument`

   - 传递给任务函数的参数zhiz
   - 可以是 int、 结构体指针、数组索引等

3. `const osThreadAttr_t *arr`

   - 指向任务属性结构体的指针，用于自定义任务行为
   - 若传 NULL, 则使用系统默认值（通常：优先级 normal， 栈大小由 RTOS 配置决定）

   ```C
   typedef struct {
       const char                   *name;        // 任务名称（调试用）
       uint32_t                     attr_bits;    // 属性位（一般设为 0）
       void                          *cb_mem;     // 控制块内存（高级用法）
       uint32_t                      cb_size;     // 控制块大小
       void                          *stack_mem;  // 自定义栈内存（可选）
       uint32_t                      stack_size;  // 栈大小（字节）
       osPriority_t                  priority;    // 任务优先级
   } osThreadAttr_t;
   ```

三、 **基本用法示例**

**示例 1: 最简创建 (使用默认属性)**

```C
void BlinkTask(void *arg){
    for(;;){
        HAL_GPIO_TogglePin(LED_GPIO, LED_PIN);
        osDelay(500);
    }
}

// 创建任务
osThreadId_t tid = osThreadNew(BlinkTask, NULL, NULL);
if (tid == NULL){
	Error_Handler();// 创建失败
}
```

**示例 2：带参数 + 自定义属性**

```C
void MotorTask(void *arg){
	uint8_t motor_id = *(uint8_t*)arg;
	for(;;){
		COntrolMotor(motor_id);
		osDelay(10);
	}
}

// 创建任务 (控制电机2)
static uint8_t motor_id = 2;
osThreadAttr_t attr = {
	.name = "Motor2Task",
	.stack_size = 1024,
	.priority = osPriorityNormal,
};
osThreadNew(MotorTask, &motor_id, &attr);
```

**示例 3： 多个任务（循环创建)**

```C
#define TASK_COUNT 4
static uint8_t task_ids[TASK_COUNT];

void WorkerTask(void *arg){
    uint8_t id = *(uint8_t*)arg;
    for(;;){
        DoWork(id);
        osDelay(100);
    }
}

// 创建 4 个任务
for (int i = 0; i < TASK_COUNT; i++){
    task_id[i] = i;
    char name[16];
    snprintf(name, sizeof(name), "Worker%d", i);
    
    osThreadAttr_t attr = {
        .name = name,
        .stack_size = 512,
        .priority = osPriorityBelowNormal,
    };
    osThreadNew(WorkerTask, &task_ids[i], &attr);
}
```

---

---

---

# **`snprintf`**

是 C 语言标准库中一个**安全、常用**的字符串格式化函数，用于将格式化的数据写入字符数组（字符串），并**自动防止缓冲区溢出**。它是 `sprintf` 的安全替代品。

一、**函数原型**

```C
#include <stdio.h>

int snprintf(char *str, size_t size, const char *format, ...);
```

**参数说明**

| 参数                 | 含义                                       |
| -------------------- | ------------------------------------------ |
| `char *str`          | 目标字符数组（缓冲区）的指针               |
| `size_t size`        | 缓冲区的最大字节数（包括结尾的 `\0`）      |
| `const char *format` | 格式控制字符串（如 `"Value: %d"`）         |
| `...`                | 可变参数列表（与 `format` 中的占位符对应） |

**返回值**

- **成功时**：返回**理论上需要的字符数**（不包括结尾 `\0`）
- **失败时**：返回负数（极少发生）

二、**基本用法示例**

**简单整数转字符串**

```C
char buf[20];
int num = 42;
snprintf(buf, sizeof(buf), "The answer is %d", num);
// buf 内容: "The answer is 42\0"
```

**浮点数格式化**

```C
char buf[32];
float pi = 3.1415926;
snprintf(buf, sizeof(buf), "Pi ≈ %.2f", pi);
// buf 内容: "Pi ≈ 3.14\0"
```

**拼接多个变量**

```C
char msg[64];
int id = 5;
float temp = 23.5;
snprintf(msg, sizeof(msg), "Sensor[%d]: %.1f°C", id, temp);
// msg 内容: "Sensor[5]: 23.5°C\0"
```

---

---

---

# **`uint8_t`**

是 C/C++ 中一种**标准、可移植的无符号 8 位整数类型**，广泛用于嵌入式系统、底层驱动、通信协议等对数据大小和内存占用敏感的场景。

一、**名称解析**

- **`u`** → **unsigned**（无符号）
- **`int`** → 整数
- **`8`** → **8 位（bit）**
- **`_t`** → **type**（类型，POSIX 和 C 标准约定）

> 所以：**`uint8_t = unsigned integer, 8 bits, type`**

二、**基本特性**

| 属性     | 值                               |
| -------- | -------------------------------- |
| 位宽     | 8 位（1 字节）                   |
| 取值范围 | `0` 到 `255`（即$$2^8$$−1 ）     |
| 内存占用 | 1 字节（`sizeof(uint8_t) == 1`） |
| 有无符号 | 无符号（不能表示负数）           |

```C
int8_t   : -128 ~ +127    （有符号）
uint8_t  :   0  ~  255    （无符号）
char     : -128 ~ +127 或 0 ~ 255（依赖编译器，不推荐用于数值）
unsigned char : 0 ~ 255   （功能同 uint8_t，但可读性差）
```

三、**定义来源**

`uint8_t` 并不是 C 语言原生关键字，而是定义在标准头文件中：

```C
#include <stdint.h>   // C99 标准（最常用）
// 或
#include <cstdint>    // C++11 标准（C++ 中使用）
```

---

---

---

# **`osMutexAcquire` **

是 **CMSIS-RTOS v2**（ARM 定义的 RTOS 标准接口）中用于**获取互斥锁（Mutex）** 的核心函数，主要用于**保护临界区**，确保多任务环境下对共享资源的**安全访问**。

一、**函数原型**

```C
#include "cmsis_os.h"

osStatus_t osMutexAcquire (
    osMutexId_t mutex_id,   // 互斥锁 ID（由 osMutexNew 创建）
    uint32_t    timeout     // 等待超时时间（单位：毫秒）
);
```

**timeout 参数详解**

| 值                                | 含义                               |
| --------------------------------- | ---------------------------------- |
| `0`                               | 非阻塞尝试：立即返回，成功或失败   |
| `>0`（如 `100`）                  | 阻塞等待最多 100ms，超时则返回错误 |
| `osWaitForever`（= `0xFFFFFFFF`） | 永久等待，直到获得锁               |

**返回值**：

- `osOK`：成功获取互斥锁
- `osErrorTimeout`：等待超时，未获得锁
- `osErrorResource`：无效的 mutex_id 或其他错误

二、**基本用法示例**

**步骤 1：创建互斥锁（通常在 main 中）**

```C
osMutexId_t xDataMutex;

int main(void) {
    osKernelInitialize();
    
    xDataMutex = osMutexNew(NULL); // 创建互斥锁
    if (xDataMutex == NULL) {
        Error_Handler();
    }
    
    osThreadNew(Task1, NULL, NULL);
    osThreadNew(Task2, NULL, NULL);
    
    osKernelStart();
}
```

**步骤 2：在任务中使用 `osMutexAcquire` / `osMutexRelease`**

```c
// 共享资源
volatile int shared_counter = 0;

void Task1(void *arg) {
    for (;;) {
        // 尝试获取锁（最多等待 100ms）
        if (osMutexAcquire(xDataMutex, 100U) == osOK) {
            // === 临界区开始 ===
            shared_counter++;
            printf("Task1: counter = %d\n", shared_counter);
            // === 临界区结束 ===
            
            osMutexRelease(xDataMutex); // 必须释放！
        } else {
            printf("Task1: Failed to acquire mutex!\n");
        }
        
        osDelay(500);
    }
}

void Task2(void *arg) {
    for (;;) {
        if (osMutexAcquire(xDataMutex, osWaitForever) == osOK) {
            shared_counter -= 2;
            printf("Task2: counter = %d\n", shared_counter);
            osMutexRelease(xDataMutex);
        }
        osDelay(700);
    }
}
```

---

---

---

# **`Semaphore`**

（信号量）是 RTOS（实时操作系统）中用于**任务间同步与资源共享控制**的核心机制之一。在 CMSIS-RTOS v2 中，它通过一组标准 API（如 `osSemaphoreNew`、`osSemaphoreAcquire` 等）提供跨平台支持。

一、**基本用法示例**

1. 创建信号量

   ```C
   osSemaphoreId_t osSemaphoreNew(
       uint32_t max_count,      // 最大计数值（二值信号量设为 1）
       uint32_t initial_count,  // 初始计数值
       const osSemaphoreAttr_t *attr  // 属性（可为 NULL）
   );
   ```

2. 获取 （P 操作 / wait)

   ```C
   osStatus_t osSemaphoreAcquire(
       osSemaphoreId_t semaphore_id,
       uint32_t timeout  // 超时时间（0=非阻塞，osWaitForever=永久等待）
   );
   ```

3. 释放 （V 操作 / signal）

   ```C
   osStatus_t osSemaphoreRelease(osSemaphoreId_t semaphore_id);
   ```

4. 删除

   ```c
   osStatus_t osSemaphoreDelete(osSemaphoreId_t semaphore_id);
   ```

二、**两种主要类型**

1. **二值信号量（Binary Semaphore)**

   - `max_count = 1`, `initial_count = 0 或 1`
   - 用于**任务同步**或**简单互斥**

   ```C
   // 示例：任务 A 等待任务 B 完成初始化
   osSemaphoreId_t init_done_sem;
   
   void TaskB(void *arg) {
       // 执行初始化...
       HAL_Delay(100); // 模拟耗时操作
       
       // 通知 TaskA：初始化完成！
       osSemaphoreRelease(init_done_sem);
   }
   
   void TaskA(void *arg) {
       // 等待初始化完成（最多等 1000ms）
       if (osSemaphoreAcquire(init_done_sem, 1000) == osOK) {
           printf("Init done! Start working...\n");
           // 继续工作...
       }
   }
   
   // main 中创建
   init_done_sem = osSemaphoreNew(1, 0, NULL); // 初始为 0（不可用）
   ```

   

2. **计数信号量（Counting Semaphore) **

   - `max_count > 1`（如 5），`initial_count <= max_count`
   - 用于**资源池管理**

   ```C
   // 示例：限制最多 2 个任务同时访问 CAN 总线
   osSemaphoreId_t can_bus_sem;
   
   void CanTask(void *arg) {
       for (;;) {
           // 请求访问 CAN 总线（最多 2 个任务可同时获得）
           if (osSemaphoreAcquire(can_bus_sem, osWaitForever) == osOK) {
               // 发送 CAN 报文
               HAL_CAN_AddTxMessage(&hcan, &txHeader, data, &mailbox);
               
               osSemaphoreRelease(can_bus_sem); // 释放资源
           }
           osDelay(10);
       }
   }
   
   // main 中创建：最多 2 个“令牌”
   can_bus_sem = osSemaphoreNew(2, 2, NULL); // 初始有 2 个可用
   ```

---

---

---

# **`extern`** 

是 C/C++ 中的一个**存储类说明符（storage class specifier）**，用于**声明一个变量或函数在其他文件中定义**，告诉编译器：“这个符号存在，但它的实际定义在别处，请链接时去找”。

它的核心作用是：**实现跨文件共享全局变量和函数**。

一、**基本语法**

1. 声明外部变量

   ```C
   extern int global_var;        // 声明：global_var 在别处定义
   extern uint8_t buffer[256];   // 声明一个外部数组
   ```

2. 声明外部函数（可省略 extern）

   ```C
   extern void init_system(void); // 声明函数（通常可省略 extern）
   // 等价于：
   void init_system(void);
   ```

二、**典型使用场景**

**场景 1：多个 .c 文件共享全局变量**

步骤 1：在 一个 .c 文件中定义变量（分配内存）

```c
// global.c
#include "global.h"

int system_state = 0;          // 定义（分配内存并初始化）
uint8_t motor_count;           // 定义（未初始化，默认为 0）
```

步骤 2：在头文件中使用 extern 声明

```c
// global.h
#ifndef __GLOBAL_H
#define __GLOBAL_H

extern int system_state;       // 声明（不分配内存）
extern uint8_t motor_count;

#endif
```

步骤 3：其他 .c 文件包含头文件即可使用

```C
// main.c
#include "global.h"

void task1(void) {
    system_state = 1;          // ✅ 可以读写
}

// motor.c
#include "global.h"

void update_motor(void) {
    motor_count++;             // ✅ 可以读写
}
```

---

---

---

# `osEventFlagsSet` 

是 **CMSIS-RTOS v2**（ARM 定义的 RTOS 标准接口）中用于**设置事件标志（Event Flags）** 的函数，常用于**任务间高效同步与通信**，特别适合“一个任务通知多个事件，另一个任务等待任意/全部事件”的场景。

一、 **核心概念**：什么是事件标志（Event Flags)?

- 一个 **32 位无符号整数**（`uint32_t`），每一位代表一个独立的“事件”
- 例如：
  - bit 0 → 数据接收完成
  - bit 1 → 电机到位
  - bit 2 → 按键按下
  - ...
- 多个事件可**同时触发**（多位为 1）

二、**基本用法**

1. 创建事件标志对象

   ```c
   osEventFlagsId_t osEventFlagsNew(const osEventFlagsAttr_t *attr);
   ```

   - 返回事件标志 ID（类似 Mutex/Semaphore 的句柄）
   - `attr` 可为 `NULL`（使用默认属性）

2. 设置事件（发送通知）

   ```C
   uint32_t osEventFlagsSet(
       osEventFlagsId_t ef_id,   // 事件标志 ID
       uint32_t flags            // 要设置的位（如 0x01 | 0x04）
   );
   ```

   - **返回值**：设置后的完整事件标志值（或错误码）
   - ✅ **可在中断服务程序（ISR）中安全调用！**

3. 等待事件（接收通知）

   ```C
   uint32_t osEventFlagsWait(
       osEventFlagsId_t ef_id,
       uint32_t flags,           // 要等待的事件位
       uint32_t options,         // 等待方式（见下文）
       uint32_t timeout          // 超时时间
   );
   ```

​	**关键参数**：options

| 选项             | 含义                               |
| ---------------- | ---------------------------------- |
| `0`（默认）      | 等待任意一个指定事件（OR 逻辑）    |
| `osFlagsWaitAll` | 等待所有指定事件（AND 逻辑）       |
| `osFlagsNoClear` | 等待后不清除事件标志（默认会清除） |

三、**用法示例**

示例 1：基本设置

```C
osEventFlagsId_t xEventGroup;

// 初始化（main 中）
xEventGroup = osEventFlagsNew(NULL);

// 在任务或 ISR 中设置事件
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin) {
    if (GPIO_Pin == KEY_PIN) {
        // 按键按下 → 设置 bit 0
        osEventFlagsSet(xEventGroup, 0x01); // ✅ 安全！可在 ISR 使用
    }
}

void MotorCallback(void) {
    // 电机到位 → 设置 bit 1
    osEventFlagsSet(xEventGroup, 0x02);
}
```

示例 2：同时设置多个事件

```C
// 通知“数据准备好 + 校验通过”
osEventFlagsSet(xEventGroup, FLAG_DATA_READY | FLAG_CRC_OK);
// 其中：
#define FLAG_DATA_READY  (1U << 0)
#define FLAG_CRC_OK      (1U << 1)
```

示例 3：主任务等待多个事件

```C
void MainTask(void *arg) {
    for (;;) {
        // 等待“按键按下 OR 电机到位”（任意一个）
        uint32_t flags = osEventFlagsWait(
            xEventGroup,
            0x01 | 0x02,     // 关注 bit0 和 bit1
            0,               // OR 逻辑
            osWaitForever
        );

        if (flags & 0x01) {
            printf("Key pressed!\n");
        }
        if (flags & 0x02) {
            printf("Motor arrived!\n");
        }
    }
}
```

示例 4：等待所有条件满足

```C
// 等待“数据准备好 AND 校验通过”
uint32_t flags = osEventFlagsWait(
    xEventGroup,
    FLAG_DATA_READY | FLAG_CRC_OK,
    osFlagsWaitAll,      // 必须两个都置位
    1000                 // 超时 1s
);
if (flags == (FLAG_DATA_READY | FLAG_CRC_OK)) {
    process_data();
}
```

---

---

---

# **`<<`**

是 C/C++ 中一种**位操作（bitwise operation）** 的写法，常用于嵌入式开发、寄存器配置、标志位设置等场景。下面我们逐层解析它的含义。

一、**拆解表达式**

| 部分 | 含义                                          |
| ---- | --------------------------------------------- |
| `1`  | 十进制数字 1                                  |
| `U`  | 后缀，表示这是一个 无符号整型（unsigned int） |
| `<<` | 左移运算符（shift left）                      |
| `0`  | 左移 0 位                                     |

二、 **计算过程**

步骤 1：

```
// 二进制表示 （32）
0000 0000 0000 0000 0000 0000 0000 0001
```

步骤 2：

```
// 1U << 0 表示：将 1U 的二进制向左移动 0 位
0000 0000 0000 0000 0000 0000 0000 0001
//(1U << 0) == 1
```

---

---

---



