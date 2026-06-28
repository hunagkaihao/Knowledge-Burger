# 快捷键

1. Ctrl + G：查询指定行
2. 





# 文件发布说明

在 Visual Studio 中，将 `Release` 整个文件夹与只将 `Release` 文件夹下的 `publish` 子文件夹打包部署到服务器，两者有着本质的区别。简单来说，**`Release` 是开发/编译目录，而 `publish` 才是专门用于生产环境部署的目录**。

以下是两者的核心区别：

### **1. 包含的文件内容不同**

- 整个 `Release` 文件夹：包含大量仅在开发、编译或调试阶段需要的文件。例如：

  - 未编译的源代码文件（如 `.cs`, `.cshtml` 等）。
  - 调试符号文件（`.pdb`），用于代码调试和错误定位。
  - Visual Studio 的临时文件、缓存和构建日志。
  - 冗余的依赖项（可能包含未使用的 NuGet 包文件）。

- `publish` 文件夹：经过 Visual Studio 的“发布（Publish）”流程后生成。它只包含运行应用程序

  绝对必需的文件，例如：

  - 编译后的二进制文件（`.dll`）。
  - 可执行文件（`.exe`）。
  - 配置文件（如 `appsettings.json`, `web.config`）。
  - 静态资源（如图片、前端文件）和必需的运行时（如果选择了独立部署）。

### **2. 安全性与性能不同**

- 整个 `Release` 文件夹：
  - **安全风险**：如果部署了包含源代码或 `.pdb` 文件的目录，可能会暴露你的业务逻辑和代码结构，构成严重的安全漏洞。
  - **性能低下**：包含大量无用文件，不仅占用服务器磁盘空间，还可能拖慢服务器的文件索引和扫描速度。
- `publish` 文件夹：
  - **安全优化**：发布过程会自动剥离不必要的调试信息（PDB 文件），并对代码进行优化（Release 模式编译），确保运行效率。
  - **干净的生产环境**：避免了将开发环境的配置文件（如本地数据库连接字符串）误覆盖到生产环境。

### **3. 部署行为与规范**

- **整个 `Release` 文件夹**：属于“复制粘贴”的粗放式部署，容易漏掉某些依赖，或者把不该上传的文件传上去。
- `publish` 文件夹：是标准的部署产物。它支持多种高级部署模式，例如：
  - **依赖框架 vs 独立部署**：你可以选择是否将 .NET 运行时打包进 `publish` 文件夹，从而决定目标服务器是否需要预装运行时。
  - **单文件发布**：可以将所有的依赖项打包成一个单独的 `.exe` 文件，让部署变得极其干净。



# 问题

1. **现象**：未能找到路径“D:\Project\After-sales service\host\host\aspnet-core\modules\DataDictionaryManagement\src\Lion.AbpPro.DataDictionaryManagement.Application.Contracts\obj\Debug\net6.0\Lion.AbpPro.DataDictionaryManagement.Application.Contracts.GeneratedMSBuildEditorConfig.editorconfig”的一部分。

   **原因**：文件路径过长。Windows 路径长度限制

   - 传统限制 ：Windows 的传统路径长度限制是 260 个字符 （包括文件名）
   - 长路径支持 ：Windows 10 1607+ 和 Windows Server 2016+ 支持更长的路径（最大 32767 字符），但需要特殊配置

   **解决方案**：文件路径改短