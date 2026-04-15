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

​	安装 MySql 目录下会出现一个 data 文件