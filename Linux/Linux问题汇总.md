# 网络不连通

## 1. 没有无线网选项

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\无无线网选项.png)

在 Ubuntu 的“设置”界面中看不到网络选项，通常是因为系统底层的网络管理服务（NetworkManager）没有正常运行。这会导致图形界面无法识别网卡，从而隐藏了网络设置入口。

你可以按照以下步骤排查和修复：

### **第一步：检查并启动 NetworkManager 服务**

这是最常见的原因。如果服务没开，图标就会消失。

1. 打开终端（快捷键 `Ctrl+Alt+T`）。

2. 输入以下命令查看服务状态：

   ```
   systemctl status NetworkManager
   ```

3. 如果显示 `inactive (dead)` 或 `failed`：

   - 输入以下命令启动它：

     ```
     sudo systemctl start NetworkManager
     ```

   - 然后让它开机自启：

     ```
     sudo systemctl enable NetworkManager
     ```

4. 再次检查：

   - 再次运行 `systemctl status NetworkManager`，确认状态变为 `active (running)`。
   - 此时右上角应该会出现网络图标，设置里也会出现网络选项。

### **第二步：检查配置文件是否被禁用**

有时候配置文件会被意外修改，导致网络管理被禁用。

1. 在终端输入：

   ```
   cat /etc/NetworkManager/NetworkManager.conf
   ```

2. 检查文件内容中是否有 

   ```
   managed=false
   ```

    这一行。

   - 如果有：请使用编辑器将其改为 `managed=true` 。

     ```
     sudo nano /etc/NetworkManager/NetworkManager.conf
     ```
     
   - 修改后保存退出（`Ctrl+O` 回车保存，`Ctrl+X`退出），然后重启服务：

     ```
     sudo systemctl restart NetworkManager
     ```

### **第三步：检查 Netplan 配置冲突**

如果你之前为了修 DNS 改过 `/etc/netplan/` 下的文件，可能会因为格式错误导致网络服务彻底崩溃。

1. 检查 Netplan 配置是否有语法错误：

   ```
   sudo netplan generate
   ```

2. 如果报错（比如提示 YAML 缩进错误），你需要重新编辑该文件修正格式（参考之前的回答，确保用空格而不是 Tab）。

3. 修正后应用配置：

   ```
   sudo netplan apply
   ```

## 2. 有无线网络选项但仍连不上网

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\有无线网选项.png)

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\有无线网选项2.png)

从你提供的最新截图来看，虽然网络管理器（NetworkManager）已经识别到了网卡 `ens33`，但依然提示 `Activation of network connection failed` 且 Ping 百度报 `Temporary failure in name resolution`。

结合你之前的操作历史（修改过 Netplan、DNS），目前的问题在于**系统内部的网络配置冲突**。简单来说，就是 Ubuntu 的“图形界面管家”和“底层配置文件”在打架，导致网卡无法正确获取 IP 或 DNS。

请按照以下三个步骤依次操作，通常能彻底解决：

### **第一步：修正 NetworkManager 配置文件**

在你的第二张截图中，我注意到 `/etc/NetworkManager/NetworkManager.conf` 文件里有一行关键配置：
`managed=false`

这行配置的意思是：“NetworkManager **不要管理**由其他工具（如 Netplan/ifupdown）配置的接口”。这就是为什么你在设置里看到网卡，但一点连接就报错的原因——它被禁止接管了。

1. 在终端输入命令编辑该文件：

   ```
   sudo nano /etc/NetworkManager/NetworkManager.conf
   ```

2. 找到 `[ifupdown]` 部分下的 `managed=false`。

3. 将其改为 **`managed=true`**。

4. 按 `Ctrl + O` 保存，回车确认，然后按 `Ctrl + X` 退出。

### **第二步：清理并重置 Netplan 配置**

既然你要用图形界面管理网络，就不需要复杂的 Netplan 静态配置文件来干扰它。我们需要把 Netplan 恢复成最简单的“自动获取”模式。

1. 查看你的 Netplan 文件名：

   ```
   ls /etc/netplan/
   ```

   （通常是 `01-network-manager-all.yaml` 或类似名字）

2. 编辑该文件（假设文件名是 `01-network-manager-all.yaml`）：

   ```
   sudo nano /etc/netplan/01-network-manager-all.yaml
   ```

3. **修改文件内容**，替换为以下标准内容（注意缩进必须用空格，不能用 Tab）：

   ```
   network:
     version: 2
     renderer: NetworkManager
     ethernets:
       ens33:
         dhcp4: true
   ```

   *(解释：这几行代码的意思是告诉系统，“版本是2，请把网络管理权完全交给 NetworkManager 图形界面”)*

4. 保存并退出（`Ctrl + O`, 回车, `Ctrl + X`）。

5. 应用配置：

   ```
   sudo netplan apply
   ```

### **第三步：重启网络服务并测试**

做完上面两步，现在的状态应该是干净的。

1. 重启 NetworkManager 服务以生效：

   ```
   sudo systemctl restart NetworkManager
   ```

2. **关键操作**：此时点击虚拟机右上角的网络图标，或者去设置里，点击 `ens33` 旁边的开关，先**关闭**再**打开**。

3. 等待几秒钟，观察是否获取到了 IP 地址。

### **第四步：验证 IP 是否获取成功**

再次输入查看 IP 的命令：

```
ip addr
```

**成功的标志**：
在 `ens33` 下面应该会出现一行以 `inet` 开头的字，例如：
`inet 192.168.xxx.xxx/24 brd ...`

如果看到了这个 `192.168...` 的 IP 地址，说明网络通了。此时再尝试 `ping www.baidu.com` 即可。

## 3. 无 IPV4 地址

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\无ipv地址.png)

从你最新的截图来看，`ip addr` 命令的输出显示网卡 `ens33` 依然只有 IPv6 地址（`inet6 fe80::...`），而**没有 IPv4 地址**（即缺少以 `192.168` 或 `172.16` 开头的 `inet` 行）。

这意味着虽然配置文件可能已经生效，但虚拟机依然无法从 VMware 的 DHCP 服务器获取到 IP 地址。这通常是因为 **Windows 宿主机上的 VMware NAT 服务或 DHCP 服务被禁用或未启动**。

### **第一步：检查虚拟网络编辑器设置**

如果服务启动了还是不行，可能是虚拟网络配置乱了。

1. 打开 VMware Workstation 软件。
2. 点击顶部菜单栏的 **“编辑” -> “虚拟网络编辑器”**。
3. 点击右下角的 **“更改设置”**（需要管理员权限）。
4. 查看列表中是否有 VMnet8 (NAT 模式)。
   - **如果没有**：点击左下角的 **“还原默认设置”**，等待几分钟让它自动重建网络。
   - **如果有**：选中 VMnet8，确保下方的 **“使用本地 DHCP 服务将 IP 地址分配给虚拟机”** 是勾选状态。

### **第二步：在虚拟机内重新获取 IP**

做完上述 Windows 端的操作后，回到 Ubuntu 虚拟机终端，尝试手动请求 IP：

1. 输入以下命令强制释放并重新获取 IP：

   ```
   sudo dhclient -v ens33
   ```

   *(注意：`-v` 会显示详细过程，如果成功，你会看到类似 `bound to 192.168.xxx.xxx` 的字样)*

2. 再次输入 `ip addr` 查看是否出现了 `inet 192.168...` 的地址。

## 4. 域名解析失败

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\域名解析失败.png)

这是一个非常经典的 Ubuntu 网络配置“坑”。你遇到的情况是：**虽然你用 `nmcli` 命令修改了配置，但系统底层的 DNS 解析服务（systemd-resolved）并没有正确更新。**

从你的截图看，`/etc/resolv.conf` 依然指向本地回环地址 `127.0.0.53`，这是 Ubuntu 特有的机制。这意味着所有的 DNS 请求都被转发给了本地的 `systemd-resolved` 服务，而这个服务可能卡住了或者没有读取到新的配置。

请按照以下二个步骤彻底解决这个问题：

### **第一步：强制重启 DNS 解析服务**

既然配置文件改了没生效，我们需要重启负责解析的服务。请在终端输入：

```
sudo systemctl restart systemd-resolved
```

然后再次尝试 Ping：

```
ping www.baidu.com
```

如果通了，问题解决。如果还是不行，继续第二步。

------

### **第二步：检查并修复软链接（最可能的根本原因）**

Ubuntu 的 `/etc/resolv.conf` 通常是一个**快捷方式（软链接）**。如果这个链接断了或者指错了地方，DNS 就会失效。

1. **查看链接状态**：
   输入命令：

   ```
   ls -l /etc/resolv.conf
   ```

   - **正常情况**应该显示指向 `../run/systemd/resolve/stub-resolv.conf`。
   - **如果显示的是普通文件**（没有 `->` 箭头），说明链接断了。

2. **重建链接（如果是断的）**：
   先删除旧文件，再建立新链接：

   ```
   sudo rm /etc/resolv.conf
   sudo ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
   ```

3. **再次重启服务**：

   ```
   sudo systemctl restart systemd-resolved
   ```

## 5. 已有 DNS 但仍 Ping 不通

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\已有dns 但ping不通.png)

从你提供的截图来看，情况非常诡异但也很典型：

1. **IP 地址正常**：`192.168.159.128`。
2. **DNS 配置已生效**：设置界面里明确显示了 `192.168.159.1`（网关）、`8.8.8.8` 和 `114.114.114.114`。
3. **系统服务正常**：`systemd-resolved` 重启了，软链接也是对的。

**既然“路”通了（有IP），“导航”也有了（有DNS），为什么还是不通？**

最大的可能性是：**你的 Ubuntu 防火墙（UFW）把网络请求给拦截了，或者网关本身就不通。**

请按照以下顺序进行“排查三连”：

### **第一步：测试网关连通性（关键诊断）**

不要直接 Ping 百度，先 Ping 你的网关（路由器）。在终端输入：

```
ping 192.168.159.1 -c 4
```

- **如果 Ping 不通（Request timeout）**：说明虚拟机连 VMware 的虚拟路由器都连不上。这通常是 VMware 的服务问题或防火墙拦截。
- **如果 Ping 通了**：说明局域网没问题，是出网被拦住了。

------

### **第二步：关闭 Ubuntu 自带防火墙（最可能的解法）**

Ubuntu 的 UFW 防火墙有时候会默认拦截所有入站甚至部分出站流量，导致即使有 IP 也无法上网。

在终端输入以下命令暂时关闭防火墙：

```
sudo ufw disable
```

## 6. 关闭防火墙，但仍上不了网

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\网络编辑配置.png)

在虚拟网络编辑器中添加 VMnet8为 NAT 模式，并按上述内容进行相关配置。配置完成后就可以正常连网了。

![](D:\Project\Knowledge-Burger\Picture\Linux\问题\网络不连通\正常联网.png)