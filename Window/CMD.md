# ipconfig/all

作用：查看主机网卡配置详细信息

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\ipconfig.png)

# route print

作用：显示终端的路由表

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\route print.png)

# arp-a

作用：用于**查看 ARP 缓存表**

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\arp-a.png)

# tracert 目标地址

作用：测试本地到远端 IP 之间经过的路径

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\tracert.png)

# nslookup 域名

作用：可以测试本机的 DNS 服务是否正常

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\nslookup.png)

# netstat -n

作用：查看主机的会话连接，目的地址和端口 

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\netstat.png)

# netstat -ano | findstr :80

作用：列出占用 80 端口的进程 ID (PID)

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\netstat -ano.png)

# taskkill /PID 你的PID /F

作用：关闭占用 80 端口的进程

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\taskkill.png)

# netsh int ipv4 show excludedportrange protocol=tcp

**作用**：查看当前系统中被保留（排除）的 TCP 端口范围

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\netsh int ipv4 show excludedportrange protocol=tcp.png)

# net stop winnat

作用：停止 Windows NAT(WinNAT) 服务。系统会释放之前被动态保留的那些 TCP/UDP 端口范围，从而让被占用的端口重新变为可用状态。

注意：需要以管理员运行cmd。

# net start winnat

作用：重新启动 Windows NAT(WinNAT) 服务

注意：需要以管理员运行cmd。

# mkdir

作用：创建目录

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\mkdir.png)

# dir

作用：查看当前目录文件信息

![dir](D:\Project\Knowledge-Burger\Picture\Window\CMD\dir.png)

# echo > 文件名

作用：创建文件

![](D:\Project\Knowledge-Burger\Picture\Window\CMD\echo.png)

# telnet [主机地址] [端口号]

作用：测试主机*192.168.31.100*上的*8081*端口是否被占用，可以使用以下命令：

```bash
telnet 192.168.31.100 8081
```

**telnet 不是内部指令**

1. 按 `Win + R` 键，输入 `optionalfeatures` 并回车。
2. 在弹出的“Windows 功能”列表中，找到 **“Telnet 客户端”**。
3. 勾选它，点击“确定”，等待系统应用更改即可。

若 Telnet 仍不可用，可用 PowerShell 内置命令：

powershell

```
Test-NetConnection 172.20.21.10 -Port 6010
```

返回 `TcpTestSucceeded: True` 表示端口开放。

# 快捷键

1. 全屏：Alt + Enter
