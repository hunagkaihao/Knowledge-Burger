# *** error 56: cannot open file Error: Flash Download failed  -  Could not load file 'D:\Keil_v5\Test\Project\Register indicator light\Objects\demo.axf'

1. 查看 Keil 底部的 "Build" 输出窗口。
2. 点击 `Project` -> `Rebuild` (或按快捷键) 重新完整编译一次项目。
3. 仔细观察输出信息，**重点查找是否有 `Error` 级别的报错**，而不仅仅是 `Warning`。
4. 如果存在编译错误（例如语法错误、头文件缺失、链接错误等），必须先解决这些错误，才能成功生成 `.axf` 文件。