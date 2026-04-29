# Lua 概述

Lua 是一种轻量小巧的脚本语言，用标准C语言编写并以源代码形式开放， 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。

Lua 是巴西里约热内卢天主教大学（Pontifical Catholic University of Rio de Janeiro）里的一个研究小组于 1993 年开发的。

# VS Code 运行 lua

**问题**：

1. VS Code 运行不了 lua

   ![](D:\Project\Knowledge-Burger\Picture\Lua\VSCode未配置lua环境.png)

在 VS Code 终端输入如下命令，查看 VS Code 中是否有配置 Lua 环境：

```lua
 $env:Path
```

如果没有，在  VS Code 终端输入如下命令：

```lua
 $env:Path += ";E:\Lua\5.1"  
```

$env:Path += "; lua.exe 文件所在路径 "

# 交互式编程

### 交互式编程

Lua 提供了交互式编程模式。我们可以在命令行中输入程序并立即查看效果。

Lua 交互式编程模式可以通过命令 lua -i 或 lua 来启用：

```lua
$ lua -i 
```

![](D:\Project\Knowledge-Burger\Picture\Lua\交互式编程.png)

# 脚本式编程

我们可以将 Lua 程序代码保存到一个以 lua 结尾的文件，并执行，该模式称为脚本式编程。