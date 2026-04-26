# 无法下载 git 仓库

![](D:\Project\Knowledge-Burger\Picture\git\无法访问.png)

​	解决办法：

**执行以下命令，全局设置 Git 使用 OpenSSL 而不是 Schannel**

```
git config --global http.sslBackend openssl
```

### **假设 URL 正确，以下是解决** `Connection was reset` **的方法**

#### **✅ 方法 1：使用** ***\*SSH 代替 HTTPS\******（推荐）**

HTTPS 在某些网络环境下容易被干扰，SSH 更稳定。

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

1. 将公钥（`~/.ssh/id_ed25519.pub`中的内容）添加到 GitHub

   ![](D:\Project\Knowledge-Burger\Picture\git\.ssh.png)

   - GitHub → Settings → SSH and GPG keys → New SSH key

2. **用 SSH 克隆**

```
git clone git@github.com:huangkaihao/Knowledge-Burger.git
```

 SSH 不走 HTTPS 端口（443），不受 TLS/代理干扰，成功率更高。

# 无法推送

<img src="D:\Project\Knowledge-Burger\Picture\git\无法推送.png" style="zoom:100%;" />

解决办法：

按照上述方式建立**公钥**，然后再按下述步骤

#### **修改 remote 为 SSH**

注意在对应仓库打开终端输入以下命令：

```
git remote set-url origin git@github.com:huangkaihao/Knowledge-Burger.git
```