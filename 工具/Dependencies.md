### **✅ 1.** ***\*Dependencies（强烈推荐 · 现代开源替代）\****

- **平台**：Windows（支持 x86/x64）

- **开源地址**：[https://github.com/lucasg/Dependencies](https://github.com/lucasg/Dependencies?spm=5176.28103460.0.0.24e26308PKXVoB)

- 特点：

  - Dependency Walker 的现代化重写（C# + GUI）
  - 支持 **Win10/Win11 及新版 API**（如 Side-by-Side、API-MS-WIN-* 重定向）
  - 显示完整的 **依赖树（含间接依赖）**
  - 高亮 **缺失的 DLL**（红色标记）
  - 支持查看 **导出函数、导入函数**
  - 提供 **命令行版本（Dependencies.exe -h）**，适合自动化
  
- **适用场景**：日常开发、部署前检查、DLL 缺失诊断

# 基本使用步骤

##### **（1）打开目标文件**

- **拖放文件**：直接将 `.exe` 或 `.dll` 拖入窗口。
- **菜单操作**：点击 `File → Open` 选择文件。

##### **（2）解析依赖树**

- **左侧面板**：以树状结构显示所有依赖的 DLL，展开可查看层级。
- **右侧面板**：
  - **Imports**：该文件调用的外部函数。
  - **Exports**：该文件提供的函数。
  - **DLL 属性**：路径、版本、架构（32/64位）。 

##### **（3） 常见问题**

| 问题                       | 解决方案                                                     |
| :------------------------- | :----------------------------------------------------------- |
| **无法识别 UWP/.NET 程序** | 使用 [ILSpy](https://github.com/icsharpcode/ILSpy)（.NET）或 [Process Explorer](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)（运行时分析） |
| **DLL 路径错误**           | 检查 `Options → Configure Search Paths` 添加自定义搜索路径   |
| **分析卡顿**               | 关闭递归扫描（递归模式可能耗时较长）                         |
