# 快捷键

| 需求                                 | 快捷键                          |
| ------------------------------------ | ------------------------------- |
| 单位切换                             | Q                               |
| 跳转到中心点                         | Ctrl + End                      |
| 设置中心参考点                       | E + F + C                       |
| 放置线                               | P + L                           |
| 放置焊盘                             | P + P                           |
| 清除过滤器（清除尺寸标注）           | Shift + C                       |
| 截断线                               | E + K                           |
| 选择栅格尺寸                         | G + G                           |
| 重新定义原点                         | E + O + S                       |
| 选择板子形状                         | D + S + D                       |
| 指令快捷键设置                       | Ctrl + 鼠标左键（点击对应指令） |
| PCB 快速查找 原理图 元器件所在位置   | T + C                           |
| PCB 元器件位置微调                   | Ctrl + 方向键                   |
| 测量距离                             | Ctrl + M                        |
| 3D 视图旋转查看                      | Shift + 右键                    |
| PCB 规则及约束编辑器（Design Rules） | D + R                           |
| 水平对齐                             | Shift + Ctrl + H                |
| 顶对齐                               | Shift + Ctrl + T                |
| 单层显示模式                         | Shift + S                       |
| 热点捕捉方式切换                     | Shift + E                       |
| 切换为电器栅格                       | V + G + E                       |
| 3D 视图翻转                          | V + B                           |
| 交互式布线连接                       | Ctrl + W                        |
|                                      |                                 |
|                                      |                                 |
|                                      |                                 |

---

## 封装名说明

**0805C**：0.08英寸 和 0.05英寸（2.0mm×1.25mm）

# 软件含义解释

1. Snap：吸附/捕捉
2. Gerber：PCB 生产厂家用来制作电路板的”施工图纸“
3. Copper：铜皮
3. Silk：丝印
3. Solder：阻焊
3. Board Outline：板框
3. 



# 知识点

1. 敷铜：手工焊接采取 十字连接（载流能力弱，散热慢）；全连接（载流能力强，散热快）

2. 信号线：6mil以上（根据生产厂商所提供的工艺要求决定，线宽、间距越大成本越低）

   电源线：15mil

3. 走线顺序：信号线走线——》电源走线——》地走线

4. 字体样式（字高:字宽 = 25:4 = 30:5 = 45:6 = 60:10） 

5. 先开孔在是丝印

4.  

# 问题

## 库文件加载失败

![](D:\Project\Knowledge-Burger\Picture\电气电子\AD\添加库失败.png)

win+R，运行`%AltiumSystemLibrary%`, 如果没有弹出来库文件所在路径，则需要添加环境变量：

![](D:\Project\Knowledge-Burger\Picture\电气电子\AD\添加库失败 问题解决.png)

如果重启 AD 后还是报错，就需要查看注册表中 library 的文件路径了，将数据路径更改为与系统变量路径一致

![](D:\Project\Knowledge-Burger\Picture\电气电子\AD\添加库失败2.png)

## 未知引脚

![](D:\Project\Knowledge-Burger\Picture\电气电子\AD\未知引脚.png)

删除 PCB 重新导入原理图

## 布线后仍是 No Net

删掉这段 `No Net` 的线 / 过孔。

按 `P → T` 开始布线，**从一个已分配网络的焊盘 / 引脚**（比如排针、芯片引脚）开始拖线。

把线连到目标焊盘 / 过孔，AD 会自动把整条线和过孔都分配到同一个网络

# 敷铜

## 敷铜完全占据整个面板

1. 打开 `Design → Rules`，展开 `Manufacturing` → 找到 `Board Outline Clearance`。

2. 新建规则（或修改默认规则），命名为 `Board_Outline_Clearance`。

3. **约束设置**：

- `Minimum Clearance`：设置你需要的板框与铜皮的间隙（比如 `20mil`）。
- 作用对象默认就是 `Board Outline` 对 `All`，无需额外修改。

4. 确认后关闭规则窗口。
5. 重新敷铜
