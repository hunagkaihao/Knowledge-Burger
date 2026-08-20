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

# 找到对应仓库

### **1. 检查远程仓库 URL 是否正确**

- 首先确认你本地配置的远程地址是否有拼写错误、大小写不匹配，或者是否缺少 `.git` 后缀。
  在终端中运行以下命令查看当前 URL：

```
git remote -v
```

- 如果地址有误，请使用正确的地址进行替换（例如替换为 SSH 地址）：

  ```
  git remote set-url origin git@github.com:hunagkaihao/Knowledge-Burger.git
  ```