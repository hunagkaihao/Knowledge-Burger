# 快捷键

1. Ctrl + G：查询指定行
2. 





# 问题

1. **现象**：未能找到路径“D:\Project\After-sales service\host\host\aspnet-core\modules\DataDictionaryManagement\src\Lion.AbpPro.DataDictionaryManagement.Application.Contracts\obj\Debug\net6.0\Lion.AbpPro.DataDictionaryManagement.Application.Contracts.GeneratedMSBuildEditorConfig.editorconfig”的一部分。

   **原因**：文件路径过长。Windows 路径长度限制

   - 传统限制 ：Windows 的传统路径长度限制是 260 个字符 （包括文件名）
   - 长路径支持 ：Windows 10 1607+ 和 Windows Server 2016+ 支持更长的路径（最大 32767 字符），但需要特殊配置

   **解决方案**：文件路径改短