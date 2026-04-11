# CMake 介绍

CMake 是个一个开源的跨平台自动化建构系统，用来管理软件建置的程序，并不依赖于某特定编译器，并可支持多层目录、多个应用程序与多个函数库。

CMake 通过使用简单的配置文件 CMakeLists.txt，自动生成不同平台的构建文件（如 Makefile、Ninja 构建文件、Visual Studio 工程文件等），简化了项目的编译和构建过程。

CMake 本身不是构建工具，而是生成构建系统的工具，它生成的构建系统可以使用不同的编译器和工具链。

# 基本工作流程

1. **编写 CMakeLists.txt 文件：** 定义项目的构建规则和依赖关系。

2. **生成构建文件：** 使用 CMake 生成适合当前平台的构建系统文件（例如 Makefile、Visual Studio 工程文件）。

3. **执行构建：** 使用生成的构建系统文件（如 `make`、`ninja`、`msbuild`）来编译项目。

   ![](D:\Project\Knowledge-Burger\Picture\CMake\基本工作流程.png)

# CMake 基础

## CMakeLists.txt 文件

CMakeLists.txt 是 CMake 的配置文件，用于定义项目的构建规则、依赖关系、编译选项等。

每个 CMake 项目通常都有一个或多个 CMakeLists.txt 文件

## 文件结构和基本语法

CMakeLists.txt 文件使用一系列的 CMake 指令来描述构建过程。常见的指令包括：

1、指定 CMake 的最低版本要求：

```cmake
cmake_minimum_required(VERSION <version>)
//例如
cmake_minimum_required(VERSION 3.10)
```

2、定义项目的名称和使用的编程语言：

```cmake
project(<project_name> [<language>...])
//例如
project(MyProject CXX)
```

3、指定要生成的可执行文件和其源文件：

```cmake
add_executable(<target> <source_files>...)
//例如
add_executable(MyExecutable main.cpp other_file.cpp)
```

4、创建一个库（静态库或动态库）及其源文件：

```cmake
add_library(<target> <source_files>...)
//例如
add_library(MyLibrary STATIC library.cpp)
```

5、链接目标文件与其他库：

```cmake
target_link_libraries(<target> <libraries>...)
//例如
target_link_libraries(MyExecutable MyLibrary)
```

6、添加头文件搜索路径：

```cmake
include_directories(<dirs>...)
//例如
include_directories(${PROJECT_SOURCE_DIR}/include)
```

7、设置变量的值：

```cmake
set(<variable> <value>...)
//例如
set(CMAKE_CXX_STANDARD 11)
```

8、设置目标属性：

```cmake
target_include_directories(TARGET target_name
                          [BEFORE | AFTER]
                          [SYSTEM] [PUBLIC | PRIVATE | INTERFACE]
                          [items1...])
//例如
target_include_directories(MyExecutable PRIVATE ${PROJECT_SOURCE_DIR}/include)
```

9、安装规则：

```cmake
install(TARGETS target1 [target2 ...]
        [RUNTIME DESTINATION dir]
        [LIBRARY DESTINATION dir]
        [ARCHIVE DESTINATION dir]
        [INCLUDES DESTINATION [dir ...]]
        [PRIVATE_HEADER DESTINATION dir]
        [PUBLIC_HEADER DESTINATION dir])
//例如
install(TARGETS MyExecutable RUNTIME DESTINATION bin)     
```

10、条件语句 (if, elseif, else, endif 命令)

```cmake
if(expression)
  # Commands
elseif(expression)
  # Commands
else()
  # Commands
endif()
//例如
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  message("Debug build")
endif()
```

11、自定义命令 (add_custom_command 命令)：

```cmake
add_custom_command(
   TARGET target
   PRE_BUILD | PRE_LINK | POST_BUILD
   COMMAND command1 [ARGS] [WORKING_DIRECTORY dir]
   [COMMAND command2 [ARGS]]
   [DEPENDS [depend1 [depend2 ...]]]
   [COMMENT comment]
   [VERBATIM]
)
//例如
add_custom_command(
   TARGET MyExecutable POST_BUILD
   COMMAND ${CMAKE_COMMAND} -E echo "Build completed."
)
```

## 变量和缓存

CMake 使用变量来存储和传递信息，这些变量可以在 CMakeLists.txt 文件中定义和使用。

变量可以分为普通变量和缓存变量。

### 变量定义与使用

**定义变量：**

```cmake
set(MY_VAR "Hello World")
```

**使用变量：**

```cmake
message(STATUS "Variable MY_VAR is ${MY_VAR}")
```

### 缓存变量

缓存变量存储在 CMake 的缓存文件中，用户可以在 CMake 配置时修改这些值。缓存变量通常用于用户输入的设置，例如编译选项和路径。

**定义缓存变量：**

```cmake
set(MY_CACHE_VAR "DefaultValue" CACHE STRING "A cache variable")
```

**使用缓存变量：**

```cmake
message(STATUS "Cache variable MY_CACHE_VAR is ${MY_CACHE_VAR}")
```

## 查找库和包

CMake 可以通过 **find_package()** 指令自动检测和配置外部库和包。

常用于查找系统安装的库或第三方库。

### find_package() 指令

基本用法：

```cmake
find_package(Boost REQUIRED)
```

指定版本：

```cmake
find_package(Boost 1.70 REQUIRED)
```

查找库并指定路径：

```cmake
find_package(OpenCV REQUIRED PATHS /path/to/opencv)
```

使用查找到的库：

```cmake
target_link_libraries(MyExecutable Boost::Boost)
```

设置包含目录和链接目录：

```cmake
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
```

### 使用第三方库

假设你想在项目中使用 Boost 库，CMakeLists.txt 文件可能如下所示：

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject CXX)

# 查找 Boost 库
find_package(Boost REQUIRED)

# 添加源文件
add_executable(MyExecutable main.cpp)

# 链接 Boost 库
target_link_libraries(MyExecutable Boost::Boost)
```

### include_directories() 和 target_include_directories()

在 CMake 中，include_directories() 和 target_include_directories() 都用于指定头文件的搜索路径，但它们的作用范围和使用方式有显著区别。

**相同点：**

- 两者都用于添加头文件的搜索路径，编译器会在这些路径中查找 #include 指令中指定的头文件。
- 两者都支持绝对路径和相对路径，相对路径是相对于当前 CMakeLists.txt 文件所在的目录。
- 两者都可以用于指定公共头文件路径（PUBLIC）、私有头文件路径（PRIVATE）或接口头文件路径（INTERFACE）。

**区别：**

| 特性                | `include_directories()`                  | `target_include_directories()`                               |
| :------------------ | :--------------------------------------- | :----------------------------------------------------------- |
| **作用范围**        | 全局作用域，影响所有目标（target）。     | 仅作用于指定的目标（target）。                               |
| **推荐使用场景**    | 适用于简单的项目或旧版 CMake 项目。      | 适用于现代 CMake 项目，推荐优先使用。                        |
| **目标关联性**      | 不直接关联到特定目标，可能影响所有目标。 | 显式关联到特定目标，避免污染其他目标。                       |
| **可维护性**        | 较差，容易导致全局路径污染。             | 较好，路径与目标绑定，逻辑清晰。                             |
| **作用域控制**      | 无法精确控制路径的作用范围。             | 可以通过 `PUBLIC`、`PRIVATE`、`INTERFACE` 精确控制路径的作用范围。 |
| **现代 CMake 推荐** | 不推荐使用，除非有特殊需求。             | 推荐使用，符合现代 CMake 的最佳实践。                        |

# CMake 构建流程

CMake 的构建流程分为几个主要步骤，从设置项目到生成和执行构建命令。

1. **创建构建目录**：保持源代码目录整洁。
2. **使用 CMake 生成构建文件**：配置项目并生成适合平台的构建文件。
3. **编译和构建**：使用生成的构建文件执行编译和构建。
4. **清理构建文件**：删除中间文件和目标文件。
5. **重新配置和构建**：处理项目设置的更改。

![](D:\Project\Knowledge-Burger\Picture\CMake\构建流程.png)

以下是详细的构建流程说明：

### 1、创建构建目录

CMake 推荐使用 **"Out-of-source"** 构建方式，即将构建文件放在源代码目录之外的独立目录中。

这样可以保持源代码目录的整洁，并方便管理不同的构建配置。

![](D:\Project\Knowledge-Burger\Picture\CMake\创建构建目录.png)

**创建构建目录：**在项目的根目录下，创建一个新的构建目录。例如，可以创建一个名为 build 的目录。

```
mkdir build
```

**进入构建目录：**进入刚刚创建的构建目录。

```
cd build
```

### 2、使用 CMake 生成构建文件

在构建目录中运行 CMake，以生成适合当前平台的构建系统文件（例如 Makefile、Ninja 构建文件、Visual Studio 工程文件等）。

**运行 CMake 配置：**在构建目录中运行 CMake 命令，指定源代码目录。源代码目录是包含 CMakeLists.txt 文件的目录。

```
cmake ..
```

如果需要指定生成器（如 Ninja、Visual Studio），可以使用 -G 选项。例如：

```
cmake -G "Ninja" ..
```

如果需要指定构建类型（如 Debug 或 Release），可以使用 -DCMAKE_BUILD_TYPE 选项。例如：

```
cmake -DCMAKE_BUILD_TYPE=Release ..
```

**检查配置结果：**CMake 会输出配置过程中的详细信息，包括找到的库、定义的选项等，如果没有错误，构建系统文件将被生成到构建目录中。

### 3、编译和构建

使用生成的构建文件进行编译和构建。

不同的构建系统使用不同的命令。

**使用 Makefile（或类似构建系统）：**如果使用 Makefile，可以运行 make 命令来编译和构建项目。

```
make
```

如果要构建特定的目标，可以指定目标名称。例如：

```
make MyExecutable
```

**使用 Ninja：**如果使用 Ninja 构建系统，运行 ninja 命令来编译和构建项目。

```
ninja
```

与 make 类似，可以构建特定的目标：

```
ninja MyExecutable
```

**使用 Visual Studio：**如果生成了 Visual Studio 工程文件，可以打开 **.sln** 文件，然后在 Visual Studio 中选择构建解决方案。

也可以使用 msbuild 命令行工具来编译：

```
msbuild MyProject.sln /p:Configuration=Release
```

### 4、清理构建文件

构建过程中生成的中间文件和目标文件可以通过清理操作删除。

**使用 Makefile：**运行 make clean 命令（如果定义了清理规则）来删除生成的文件。

```
make clean
```

**使用 Ninja：**运行 ninja clean 命令（如果定义了清理规则）来删除生成的文件。

```
ninja clean
```

**手动删除：**可以手动删除构建目录中的所有文件，但保留源代码目录不变。例如：

```
rm -rf build/*
```

### 5、重新配置和构建

如果修改了 CMakeLists.txt 文件或项目设置，可能需要重新配置和构建项目。

**重新运行 CMake 配置：**在构建目录中重新运行 CMake 配置命令。

```
cmake ..
```

**重新编译：**使用构建命令重新编译项目。

```
make
```

# CMake 构建实例

CMake 构建步骤如下：

1. **创建 `CMakeLists.txt` 文件**：定义项目、目标和依赖。
2. **创建构建目录**：保持源代码目录整洁。
3. **配置项目**：使用 CMake 生成构建系统文件。
4. **编译项目**：使用构建系统文件编译项目。
5. **运行可执行文件**：执行生成的程序。
6. **清理构建文件**：删除中间文件和目标文件。

假设我们有一个简单的 C++ 项目，包含一个主程序文件和一个库文件，我们将使用 CMake 构建这个项目。

我们的项目结构如下：

```
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── mylib.cpp
└── include/
    └── mylib.h
```

- `main.cpp`：主程序源文件。
- `mylib.cpp`：库源文件。
- `mylib.h`：库头文件。
- `CMakeLists.txt`：CMake 配置文件。

### 1、创建 CMakeLists.txt 文件

在 MyProject 目录下创建 CMakeLists.txt 文件。

CMakeLists.txt 文件用于配置 CMake 项目。

**CMakeLists.txt 文件内容：**

## 实例

```cmake
cmake_minimum_required(VERSION 3.10)  # 指定最低 CMake 版本

project(MyProject VERSION 1.0)      # 定义项目名称和版本

\# 设置 C++ 标准为 C++11
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

\# 添加头文件搜索路径
include_directories(${PROJECT_SOURCE_DIR}/include)

\# 添加源文件
add_library(MyLib src/mylib.cpp)     # 创建一个库目标 MyLib
add_executable(MyExecutable src/main.cpp) # 创建一个可执行文件目标 MyExecutable

\# 链接库到可执行文件
target_link_libraries(MyExecutable MyLib)
```

**说明：**

- **`cmake_minimum_required(VERSION 3.10)`**：指定 CMake 的最低版本为 3.10。
- **`project(MyProject VERSION 1.0)`**：定义项目名称为 `MyProject`，版本为 1.0。
- **`set(CMAKE_CXX_STANDARD 11)`**：指定 C++ 标准为 C++11。
- **`include_directories(${PROJECT_SOURCE_DIR}/include)`**：指定头文件目录。
- **`add_library(MyLib src/mylib.cpp)`**：创建一个名为 `MyLib` 的库，源文件是 `mylib.cpp`。
- **`add_executable(MyExecutable src/main.cpp)`**：创建一个名为 `MyExecutable` 的可执行文件，源文件是 `main.cpp`。
- **`target_link_libraries(MyExecutable MyLib)`**：将 `MyLib` 库链接到 `MyExecutable` 可执行文件。

### 2、创建构建目录

为了保持源代码目录的整洁，我们将在项目根目录下创建一个单独的构建目录。

**创建构建目录**

打开终端，进入 MyProject 目录，然后创建构建目录：

```
mkdir build
cd build
```

### 3、配置项目

在构建目录中使用 CMake 配置项目。

这将生成适合平台的构建系统文件（如 Makefile）。

**运行 CMake 配置**

在构建目录中运行 CMake 配置命令：

```
cmake ..
```

- **`cmake ..`**：`..` 指向源代码目录，即包含 `CMakeLists.txt` 文件的目录。CMake 将读取 `CMakeLists.txt` 文件并生成构建系统文件。

### 4、编译项目

使用生成的构建系统文件编译项目。根据生成的构建系统文件类型，使用相应的构建命令。

**使用 Makefile**

如果生成了 Makefile（在大多数类 Unix 系统中默认生成），可以使用 make 命令进行编译：

```
make
```

- **`make`**：编译项目并生成可执行文件 `MyExecutable`。

### 5、运行可执行文件

编译完成后，可以运行生成的可执行文件。

**运行可执行文件**

在构建目录中，运行 MyExecutable：

```
./MyExecutable
```

### 6、清理构建文件

清理构建文件以删除生成的中间文件和目标文件。

**使用 make clean**

如果在 CMakeLists.txt 中定义了清理规则，可以使用 **make clean** 命令：

```
make clean
```

- **`make clean`**：删除中间文件和目标文件。

**手动删除**

如果没有定义清理规则，可以手动删除构建目录中的所有文件：

```
rm -rf build/*
```

# CMake 高级特性

CMake 高级特性允许我们更灵活地管理和配置 CMake 项目，以适应复杂的构建需求和环境。

本文将从以下几方面展开说明：

1. **自定义 CMake 模块和脚本**：创建自定义模块和脚本以简化构建过程。
2. **构建配置和目标**：使用多配置生成器和定义多个构建目标。
3. **高级查找和配置**：灵活地查找包和配置构建选项。
4. **生成自定义构建步骤**：添加自定义命令和目标以执行额外的构建操作。
5. **跨平台和交叉编译**：支持不同平台的构建和交叉编译。
6. **目标属性和配置**：设置和修改目标的属性，以满足特定需求。

------

## 1、自定义 CMake 模块和脚本

### 1.1 自定义 CMake 模块

CMake 允许你创建和使用自定义模块，以简化常见的构建任务。

自定义模块通常包含自定义的 CMake 脚本和函数。

**创建自定义模块：**

- 在项目目录下创建一个 `cmake/` 目录，用于存放自定义 CMake 模块。

- 在 `cmake/` 目录下创建一个 `MyModule.cmake` 文件。

- 在 CMakeLists.txt 文件中包含自定义模块：

  ```cmake
  list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
  include(MyModule)
  ```

- `list(APPEND CMAKE_MODULE_PATH ...)` 用于扩展 CMake 的模块搜索路径。
- `include(MyModule)` 用于加载并执行指定的 CMake 模块文件。

**自定义模块示例 (MyModule.cmake)：**

## 实例

```cmake
function(my_custom_function)
 message(STATUS "This is a custom function!")
endfunction()
```

在 CMakeLists.txt 中调用自定义函数：

```
my_custom_function()
```

### 1.2 使用自定义 CMake 脚本

自定义 CMake 脚本允许你执行自定义配置操作，灵活处理复杂的构建要求。

**创建自定义脚本：**

- 在项目中创建一个脚本文件（例如 `config.cmake`）。
- 在脚本中编写 CMake 指令。

在 CMakeLists.txt 文件中调用脚本：

```
include(${CMAKE_SOURCE_DIR}/config.cmake)
```

------

## 2、构建配置和目标

### 2.1 多配置生成器

CMake 支持多种构建配置（如 Debug、Release）。

多配置生成器允许你在同一构建目录中同时生成不同配置的构建。

**指定配置类型**

在 CMakeLists.txt 中设置默认配置：

```
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Build type")
```

使用 Visual Studio：

在 Visual Studio 中选择构建配置（Debug 或 Release）。

### 2.2 构建目标

你可以定义多个构建目标，每个目标可以有不同的构建设置和选项。

添加多个目标：

```
add_executable(MyExecutable1 src/main1.cpp)
add_executable(MyExecutable2 src/main2.cpp)
```

设置目标属性：

```
set_target_properties(MyExecutable1 PROPERTIES COMPILE_DEFINITIONS "DEBUG")
set_target_properties(MyExecutable2 PROPERTIES COMPILE_DEFINITIONS "RELEASE")
```

------

## 3、高级查找和配置

### 3.1 查找包的高级用法

find_package() 指令可以用于查找和配置复杂的第三方库和包。

查找包的高级选项：

```
find_package(Boost REQUIRED COMPONENTS filesystem system)
```

设置查找路径：

```
set(BOOST_ROOT "/path/to/boost")
find_package(Boost REQUIRED)
```

### 3.2 配置文件和构建选项

你可以通过 CMake 配置文件来控制构建选项和配置。

配置选项：

```
configure_file(config.h.in config.h)
```

配置文件 (config.h.in)：

```
#define VERSION "@PROJECT_VERSION@"
```

在源文件中包含配置文件：

```
#include "config.h"
```

------

## 4、生成自定义构建步骤

### 4.1 自定义命令

CMake 允许你添加自定义构建命令，以便在构建过程中执行额外的操作。

添加自定义命令：

```
add_custom_command(
  OUTPUT ${CMAKE_BINARY_DIR}/generated_file.txt
  COMMAND ${CMAKE_COMMAND} -E echo "Generating file" > ${CMAKE_BINARY_DIR}/generated_file.txt
  DEPENDS ${CMAKE_SOURCE_DIR}/input_file.txt
)
```

添加自定义目标：

```
add_custom_target(generate_file ALL
  DEPENDS ${CMAKE_BINARY_DIR}/generated_file.txt
)
```

### 4.2 自定义目标

自定义目标可以用来执行自定义构建步骤，如生成代码、处理资源等。

创建自定义目标：

```
add_custom_target(my_target
  COMMAND ${CMAKE_COMMAND} -E echo "Running custom target"
  DEPENDS some_dependency
)
```

在构建过程中执行目标：

```
cmake --build . --target my_target
```

------

## 5、跨平台和交叉编译

### 5.1 跨平台构建

CMake 支持多平台构建，允许你为不同操作系统生成适当的构建文件。

指定平台：

```
cmake -DCMAKE_SYSTEM_NAME=Linux ..
```

### 5.2 交叉编译

CMake 支持交叉编译，即为不同的架构或平台构建项目。

指定工具链文件：

```
cmake -DCMAKE_TOOLCHAIN_FILE=/path/to/toolchain.cmake ..
```

工具链文件示例 (toolchain.cmake)：

```
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)
```

------

## 6、目标属性和配置

### 6.1 目标属性

设置和修改目标的属性，如编译选项、链接选项等。

设置编译选项：

```
set_target_properties(MyExecutable PROPERTIES COMPILE_OPTIONS "-Wall")
```

设置链接选项：

```
set_target_properties(MyExecutable PROPERTIES LINK_FLAGS "-L/path/to/lib")
```

### 6.2 自定义编译和链接选项

为特定目标设置自定义的编译和链接选项。

设置编译选项：

```
target_compile_options(MyExecutable PRIVATE -Wall -Wextra)
```

设置链接选项：

```
target_link_options(MyExecutable PRIVATE -L/path/to/lib)
```

# CMake 实战练习

本文将演示如何使用 CMake 管理一个中等复杂度的项目，从创建项目到编译和运行的整个过程，涵盖了从基本配置到高级特性的实际应用。

实战内容如下：

1. **创建 `CMakeLists.txt` 文件**：定义项目、库、可执行文件和测试。
2. **编写源代码和测试**：编写代码和测试文件。
3. **创建构建目录**：保持源代码目录整洁。
4. **配置项目**：生成构建系统文件。
5. **编译项目**：生成目标文件。
6. **运行可执行文件**：执行程序。
7. **运行测试**：验证功能正确性。
8. **使用自定义命令和目标**：执行额外操作。
9. **跨平台和交叉编译**：支持不同平台和架构。

### 构建一个简单的 C++ 项目

假设我们有一个项目，包含一个主程序和一个库，库中有两个不同的功能模块。

项目结构如下：

```
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── lib/
│   │   ├── module1.cpp
│   │   ├── module2.cpp
│   ├── include/
│       └── mylib.h
└── tests/
    ├── test_main.cpp
    └── CMakeLists.txt
```

------

## 1、创建 CMakeLists.txt 文件

### 1.1 根目录 CMakeLists.txt 文件

在 MyProject 根目录下创建一个 CMakeLists.txt 文件：

## 实例

```
cmake_minimum_required(VERSION 3.10)  # 指定最低 CMake 版本
project(MyProject VERSION 1.0)      # 定义项目名称和版本

\# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

\# 包含头文件路径
include_directories(${PROJECT_SOURCE_DIR}/src/include)

\# 添加子目录
add_subdirectory(src)
add_subdirectory(tests)
```

### 1.2 src 目录 CMakeLists.txt 文件

在 src 目录下创建一个 CMakeLists.txt 文件：

## 实例

```
\# 创建库目标
add_library(MyLib STATIC
  lib/module1.cpp
  lib/module2.cpp
)

\# 指定库的头文件
target_include_directories(MyLib PUBLIC ${CMAKE_SOURCE_DIR}/src/include)

\# 创建可执行文件目标
add_executable(MyExecutable main.cpp)

\# 链接库到可执行文件
target_link_libraries(MyExecutable PRIVATE MyLib)
```

### 1.3 tests 目录 CMakeLists.txt 文件

在 tests 目录下创建一个 CMakeLists.txt 文件：

## 实例

```
\# 查找 GTest 包
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

\# 创建测试目标
add_executable(TestMyLib test_main.cpp)

\# 链接库和 GTest 到测试目标
target_link_libraries(TestMyLib PRIVATE MyLib ${GTEST_LIBRARIES})
```

------

## 2、编写源代码和测试

以下是各个文件的代码：

### 2.1 src/main.cpp 文件代码

## 实例

```
\#include <iostream>
\#include "mylib.h"

int main() {
  std::cout << "Hello, CMake!" << std::endl;
  return 0;
}
```

### 2.2 src/lib/module1.cpp 文件代码

## 实例

```
#include "mylib.h"

// Implementation of module1
```

### 2.3 src/lib/module2.cpp 文件代码

## 实例

```
#include "mylib.h"

// Implementation of module2
```

### 2.4 src/include/mylib.h 文件代码

## 实例

```
#ifndef MYLIB_H
#define MYLIB_H

// Declarations of module functions

#endif // MYLIB_H
```

### 2.5 tests/test_main.cpp 文件代码

## 实例

```
#include <gtest/gtest.h>

// Test cases for MyLib
TEST(MyLibTest, BasicTest) {
  EXPECT_EQ(1, 1);
}
```

------

## 3、创建构建目录

在项目根目录下创建一个构建目录：

```
mkdir build
cd build
```

------

## 4、配置项目

在构建目录中运行 CMake 以配置项目：

```
cmake ..
```

------

## 5、编译项目

使用生成的构建系统文件进行编译，假设 build 文件夹中生成了 Makefile：

```
cmake --build .
```

------

## 6、运行可执行文件

编译完成后，可以运行生成的可执行文件：

```
./MyExecutable
```

------

## 7、运行测试

使用生成的测试目标进行测试：

```
./TestMyLib
```

------

## 8、使用自定义命令和目标

### 8.1 自定义命令

在 src/CMakeLists.txt 文件中添加自定义命令：

```
add_custom_command(
    TARGET MyExecutable
    POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E echo "Build complete!"
)
```

### 8.2 自定义目标

在 src/CMakeLists.txt 文件中添加自定义目标：

```
add_custom_target(run
    COMMAND ${CMAKE_BINARY_DIR}/MyExecutable
    DEPENDS MyExecutable
)
```

运行自定义目标：

```
make run
```

------

## 9、跨平台和交叉编译

### 9.1 指定平台

如果需要指定平台进行构建，可以在运行 CMake 时指定平台：

```
cmake -DCMAKE_SYSTEM_NAME=Linux ..
```

### 9.2 使用工具链文件

创建一个工具链文件 toolchain.cmake：

```
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)
```

使用工具链文件进行构建：

```
cmake -DCMAKE_TOOLCHAIN_FILE=toolchain.cmake ..
```