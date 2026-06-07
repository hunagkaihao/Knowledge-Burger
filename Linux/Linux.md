# 什么是Linux?

与大家熟知的 Windows 操作系统软件一样, Linux也是一个操作系统软件。但是与Windows不同的是, Linux是一套开放源代码程序的,并可以自由传播的类UNIX操作系统软件(UNIX系统是 Linux系统的前身,具备很多优秀特性)。其在设计之初,就是基于 Intel x86系列CPU架构计算机的。它是一个基于 POSIX的多用户、多任务并且支持多线程和多CPU的操作系统。

# 什么是CentOS?

社区企业版操作系统（Community Enterprise Operating System ，CentOS）是 Linux 发行版之一，它是来自于 Red Hat Enterprise Linux 依照开放源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以 CentOS 替代商业版的 Red Hat Enterprise Linux使用。CentOS 于 Red Hat Linux 不同之处在于 CentOS 并不包含封闭的源代码软件，可以开源免费使用，得到运维人员、企业、程序员的青睐，CentOS 发行版操作系统是目前企业使用最多的系统之一，2016年12月12日，CentOS基于 Red Hat Enterprise Linux 的 CentOS Linux7 (1611) 系统正式对外发布。

# Linux 系统目录结构

![](D:\Project\Knowledge-Burger\Picture\Linux\学习\目录结构.png)

**树状目录结构**：

![](D:\Project\Knowledge-Burger\Picture\Linux\学习\树状目录结构.png)

![](D:\Project\Knowledge-Burger\Picture\Linux\学习\树状目录结构2.png)

| 目录               | 作用详解                                                     | 常见文件 / 示例                               |
| :----------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| `/`                | 根文件系统，所有挂载点与路径的起点。包含系统必须的子目录与入口结构。 | 无具体数据文件，只有子目录结构。              |
| `/bin`             | 系统启动和单用户模式下必须的基础命令，所有用户可执行。       | `ls`, `cp`, `mv`, `rm`, `bash`                |
| `/sbin`            | 系统管理与维护命令，面向 root。                              | `fsck`, `reboot`, `shutdown`                  |
| `/usr`             | 用户级程序与库的主集合，包含大部分系统软件、文档与工具。     | `/usr/bin`, `/usr/lib`, `/usr/share`          |
| `/usr/bin`         | 常规用户程序的主要放置目录。                                 | `python3`, `vim`, `git`                       |
| `/usr/sbin`        | 管理工具的扩展集合。                                         | `useradd`, `apache2ctl`                       |
| `/usr/lib`         | `/usr` 内程序所依赖的动态库与模块。                          | 各类 `.so` 动态库                             |
| `/usr/local`       | 本机安装或编译软件的独立区域。                               | `/usr/local/bin`, `/usr/local/lib`            |
| `/lib` 或 `/lib64` | 系统启动核心库、动态链接器所在位置。                         | `libc.so.6`, `ld-linux.so`                    |
| `/etc`             | 系统级配置中心，统一存放所有服务和系统配置。                 | `passwd`, `group`, `fstab`, `ssh/sshd_config` |
| `/home`            | 普通用户的个人主目录集合。                                   | `/home/user/.bashrc`, `/home/user/Documents`  |
| `/root`            | root 用户的主目录。                                          | `/root/.ssh/`                                 |
| `/var`             | 频繁变化的数据：日志、缓存、数据库运行文件等。               | `/var/log`, `/var/lib`, `/var/cache`          |
| `/var/log`         | 所有系统与服务日志的集中位置。                               | `syslog`, `auth.log`, `kern.log`              |
| `/var/cache`       | 应用和包管理器缓存。                                         | `/var/cache/apt/`                             |
| `/var/lib`         | 服务的持久化状态数据。                                       | `mysql/`, `docker/`                           |
| `/tmp`             | 程序运行时的临时文件区，随时可清理。                         | 临时文件、socket 路径                         |
| `/boot`            | 启动所需文件：内核、initramfs、引导配置。                    | `vmlinuz-*`, `initrd.img`, `grub/grub.cfg`    |
| `/dev`             | 设备节点集合，文件即设备。                                   | `/dev/sda`, `/dev/null`, `/dev/tty0`          |
| `/proc`            | 由内核提供的虚拟文件系统，展示系统与进程的实时信息。         | `/proc/cpuinfo`, `/proc/<pid>/`               |
| `/sys`             | 设备、驱动、内核子系统的状态接口。                           | `/sys/class/net/`, `/sys/block/`              |
| `/mnt`             | 手动挂载的临时挂载点。                                       | `/mnt/usb`                                    |
| `/media`           | 自动挂载外接设备的默认位置。                                 | `/media/user/USB_DRIVE`                       |
| `/run`             | 运行时数据存放点，重启后清空。                               | `*.pid`, 运行状态 socket 文件                 |

# 文件基本属性

在 Linux 中第一个字符代表这个文件是目录、文件或链接文件等等。

- 当为 **d** 则是目录
- 当为 **-** 则是文件；
- 若是 **l** 则表示为链接文档(link file)；
- 若是 **b** 则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；
- 若是 **c** 则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。

接下来的字符中，以三个为一组，且均为 **rwx** 的三个参数的组合。其中， **r** 代表可读(read)、 **w** 代表可写(write)、 **x** 代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号 **-** 而已。

![](D:\Project\Knowledge-Burger\Picture\Linux\学习\文件属性.png)

每个文件的属性由左边第一部分的 10 个字符来确定（如下图）。

![](D:\Project\Knowledge-Burger\Picture\Linux\学习\文件属性2.png)

从左至右用 **0-9** 这些数字来表示。

第 **0** 位确定文件类型，第 **1-3** 位确定属主（该文件的所有者）拥有该文件的权限。

第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。

其中，第 **1、4、7** 位表示读权限，如果用 **r** 字符表示，则有读权限，如果用 **-** 字符表示，则没有读权限；

第 **2、5、8** 位表示写权限，如果用 **w** 字符表示，则有写权限，如果用 **-** 字符表示没有写权限；第 **3、6、9** 位表示可执行权限，如果用 **x** 字符表示，则有执行权限，如果用 **-** 字符表示，则没有执行权限。
