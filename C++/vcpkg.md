## vcpkg 简介与使用

**vcpkg** 是一个由 Microsoft 和 C++ 社区维护的开源 C/C++ 包管理器，支持在 Windows、macOS 和 Linux 上运行。它旨在简化 C/C++ 库的管理，提供一致的跨平台体验，并支持多种构建系统如 CMake 和 MSBuild。

核心功能

vcpkg 提供了丰富的功能，包括：

**跨平台支持**：支持 Windows、macOS 和 Linux，适用于多操作系统开发。**版本控制**：通过清单文件管理依赖项版本，确保构建的可重复性。**二进制缓存**：支持缓存生成的包，减少重复构建。**自定义注册表**：允许用户创建和管理自定义库。**离线支持**：通过资产缓存功能，在离线环境中也能正常工作。

安装与配置

在 Windows 上安装 vcpkg 的步骤如下：

```bash
# 克隆 vcpkg 仓库
git clone https://github.com/Microsoft/vcpkg

# 进入 vcpkg 目录
cd vcpkg

# 运行安装脚本
bootstrap-vcpkg.bat
```

安装完成后，可以通过以下命令将 vcpkg 集成到 Visual Studio：

```bash
.\vcpkg integrate install
```

使用示例

安装库时，可以通过以下命令指定目标架构：

```bash
# 安装 jsoncpp 库
vcpkg install jsoncpp:x64-windows
```

查看已安装的库：

```bash
.\vcpkg list
```

卸载库：

```bash
.\vcpkg remove zlib:x64-windows
```

优势

vcpkg 的主要优势包括：

- **统一的版本管理**：避免不同库版本冲突。

- **灵活的构建方式**：支持从源代码构建或使用预生成的二进制文件。
- **跨平台一致性**：无需为每个操作系统寻找不同的包管理器。
- **与构建系统集成**：自动与 CMake 和 MSBuild 集成，简化依赖管理。

注意事项

vcpkg 默认安装的是 x86 架构的库，若需安装其他架构的库，需要通过 *--triplet* 参数指定。例如：

```bash
\vcpkg install openssl --triplet x64-windows
```

vcpkg 是一个强大的工具，适用于从个人项目到企业级开发的各种场景。通过其灵活的功能和广泛的支持，开发者可以更高效地管理 C/C++ 项目的依赖项。

# 问题

1. 如果 C++ 项目还是没有显示对应的库函数，按以下方法进行配置

   | 设置项                      | 值（假设 vcpkg 在 `E:\vcpkg`）           |
   | --------------------------- | ---------------------------------------- |
   | C/C++ → 常规 → 附加包含目录 | `E:\vcpkg\installed\x64-windows\include` |
   | 链接器 → 常规 → 附加库目录  | `E:\vcpkg\installed\x64-windows\lib`     |
   | 链接器 → 输入 → 附加依赖项  | `jsoncpp.lib`                            |