# 查询空 IP

![](D:\Project\Knowledge-Burger\Picture\Window\扫描IP图标.png)

点击 扫描 按钮

![](D:\Project\Knowledge-Burger\Picture\Window\IP扫描结果.png)

可以看到对应网络下，已有设备占用 IP 的情况。

1. 打开 CMD（命令提示符）。
2. 输入：`ping 192.168.0.200 -t`
3. 观察 1-2 分钟
   - 如果有 `来自...的回复`：**绝对不能用**，有人在用。
   - 如果是 `请求超时`：**风险依然存在**（可能是对方开了防火墙）。
   - 如果是 `无法访问目标主机`：说明真的没人用，**相对安全**。

![](D:\Project\Knowledge-Burger\Picture\Window\IP访问情况.png)