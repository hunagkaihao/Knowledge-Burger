## 一、PLC到底是什么？

PLC，全称**可编程逻辑控制器**，说白了就是一台专门为工业环境设计的“工控电脑”。

它负责接收传感器信号，按照你写好的程序做逻辑判断，然后控制电机、阀门、气缸执行动作。

自动化生产线、机器人、电梯、智能仓储，但凡能动的东西，背后基本都有PLC。

## 二、硬件长什么样？

一个完整的PLC系统由这几块组成：

**CPU：大脑，执行程序、处理数据。**

**存储器：ROM存系统，RAM存用户程序，EEPROM存备份。**

**I/O模块：这是PLC和外界打交道的地方。**

**通信接口：RS485、以太网、PROFIBUS，用来和触摸屏、上位机、变频器聊天。**

**电源模块：把220V交流电转成24V直流电，给PLC和传感器供电。**

## 三、PLC怎么工作的？

PLC的运行逻辑很简单，就四个步骤循环执行：

输入采样 → 程序执行 → 输出刷新 → 通信自检

这叫“扫描周期”，通常几毫秒到几百毫秒。PLC就这么一圈一圈地转，永不停歇。

## 四、主流品牌有哪些？

市面上PLC品牌很多，新手优先选市场占有率高的：

- [西门子](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=西门子&zhida_source=entity)（德国）：S7-1200、S7-1500，工业自动化标杆
- [罗克韦尔](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=罗克韦尔&zhida_source=entity)（美国）：CompactLogix、ControlLogix，北美霸主
- [三菱](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=三菱&zhida_source=entity)（日本）：FX系列、Q系列，日系代表，资料丰富
- [欧姆龙](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=欧姆龙&zhida_source=entity)（日本）：CP1E、NJ系列，运动控制强
- [施耐德](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=施耐德&zhida_source=entity)（法国）：Modicon M580，电力行业见长
- [汇川](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=汇川&zhida_source=entity)（中国）：H5U、AM系列，国产性价比之王

建议：新手从西门子S7-200 SMART或三菱FX系列入手，资料多、社区活跃。

## 五、核心元件和指令

- 输入继电器（X/I）：物理按钮的映射
- 输出继电器（Y/Q）：控制电机的映射
- 辅助继电器（M）：中间变量，像编程里的flag
- 定时器（T）：延时控制，比如电机启动后等5秒再动作
- 计数器（C）：脉冲计数，比如数产品数量
- 数据寄存器（D）：存数据，比如温度值

**常用指令：**

- 位逻辑：常开常闭、线圈输出、自锁互锁
- 定时器：TON（通电延时）、TOF（断电延时）
- 计数器：CTU（加计数）、CTD（减计数）
- 比较指令：CMP、>I、<R，用来做阈值判断
- 数学运算：加减乘除，处理模拟量数据

## 六、编程语言怎么选？

PLC编程语言主要有5种，新手优先学[梯形图](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=梯形图&zhida_source=entity)：

- 梯形图（LD）：最常用，和电气原理图长得像，电工上手最快。
- [指令表](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=指令表&zhida_source=entity)（IL）：底层文本语言，像汇编，现在用得少了。
- [结构化文本](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=结构化文本&zhida_source=entity)（ST）：高级语言，像C或Pascal，适合复杂算法。
- [功能块图](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=功能块图&zhida_source=entity)（FBD）：图形化模块化，像搭积木。
- [顺序功能图](https://zhida.zhihu.com/search?content_id=272050997&content_type=Article&match_order=1&q=顺序功能图&zhida_source=entity)（SFC）：适合描述多步骤流程，比如流水线工序。

**建议：新手先死磕梯形图，把逻辑搞通，再学ST处理复杂功能。**