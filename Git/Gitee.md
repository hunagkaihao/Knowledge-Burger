# 问题

## 1. 推送代码报无法连接到服务器。

![](D:\Project\Knowledge-Burger\Picture\gitee\无法连接到服务器.png)

 **给 Gitee 域名设置“不通过代理”**

   ```cmd
   git config --global http.https://gitee.com.proxy ""
   ```

**确保你已经取消了全局代理（如果之前设过）**

为了避免之前的全局代理继续干扰，请再次运行这两条命令来彻底关闭代理：

```cmd
git config --global --unset http.proxy
git config --global --unset https.proxy
```

**重新尝试推送**

```cmd
git push origin develop
```
