查看 mysql 是否安装成功（建议用管理员身份运行）

```mysql
mysql --version
```

![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\version.png)

安装 mysql

```mysql
mysqld --install
```

![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\install.png)

启动服务

```mysql
net start mysql
```

![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\start.png)

登录验证

```mysql
mysql -u root -p
```

![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\登录验证.png)

创建一个数据库（数据库名根据项目不同命名）

```mysql
CREATE DATABASE chenghuiecs CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

退出 MySQL

```mysql
EXIT;
```

![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\exit.png)





# 问题

1. ![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\mysql不是内部命令.png)

​	添加环境变量![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\系统变量.png)

2. MySQL 服务启动失败

   ![](D:\Project\Knowledge-Burger\Picture\数据库\Mysql\start fail.png)

​	初始化数据库

```mysql
mysqld --initialize-insecure
```

​	安装 MySql 目录（如 `cd "C:\Program Files\MySQL\MySQL Server 8.0\bin"`）下会出现一个 data 文件



3. Unable to connect to any of the specified MySQL hosts，10055

### **调整 MySQL 配置：增加最大连接数**

检查 MySQL 服务端的连接数限制。

- 打开 MySQL 配置文件（Windows 下为 `my.ini`），在 `[mysqld]` 节点下检查或添加 `max_connections` 参数。默认值通常为 151，对于高并发调度系统可能不够。
- 建议将其调大，例如设置为 500 或 1000，然后重启 MySQL 服务。

### **调整 Windows 注册表：扩大动态端口范围**

Windows 系统默认分配的动态端口数量有限，高并发下容易耗尽。可以通过修改注册表来扩大可用端口范围：

1. 按下 `Win + R`，输入 `regedit` 打开注册表编辑器。
2. 导航到路径：`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters`。
3. 在右侧空白处，新建一个 `DWORD (32位) 值`，命名为 `MaxUserPort`，将其值设置为 `65534`（十进制）。
4. 修改完成后，**重启计算机**使配置生效。

### **调整 Windows 注册表：缩短 TIME_WAIT 时间**

当数据库连接关闭后，系统会将其保留在 `TIME_WAIT` 状态一段时间（默认 240 秒），在此期间该端口不能被重用。缩短这个时间可以加快端口回收：

1. 同样在注册表路径 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters` 下。
2. 新建一个 `DWORD (32位) 值`，命名为 `TcpTimedWaitDelay`。
3. 将其值设置为 `30`（十进制，表示 30 秒，最小建议值为 30）。
4. 修改完成后，同样需要**重启计算机**生效。