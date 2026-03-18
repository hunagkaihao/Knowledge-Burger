# 前言

**ST-LINK** **V2**是意法半导体（**ST**Microelectronics）官方推出的调试与**烧录**工具，专为STM8和STM32系列MCU设计。

ST-LINK是专门针对意法半导体STM8和STM32系列芯片的仿真器。ST-LINK /V2指定的SWIM标准接口和JTAG / SWD标准接口，其主要功能有

1. 编程功能：可烧写FLASH ROM、EEPROM、AFR等；
2. 仿真功能：支持全速运行、单步调试、断点调试等各种调试方法，可查看IO状态，变量数据等；
3. 仿真性能：采用USB2.0接口进行仿真调试，单步调试，断点调试，反应速度快；
4. 编程性能：采用USB2.0接口，进行SWIM / JTAG / SWD下载，下载速度快；

**ST-Link的几个版本差异**：ST-Link可以分为3大版本：ST-LINK、ST-LINK/V2 和 STLINK-V3。

**硬件**：

![](D:\学习笔记\Knowledge-Burger\Picture\stlink\烧录器种类.jpeg)





# 功能介绍

#### LED状态说明

- 闪烁红色：ST-LINK/V2连接到计算机后，第一次USB枚举过程
- 红色：ST-LINK/V2与计算机已建立连接
- 闪烁绿色/红色：目标板和计算机在进行数据交换
- 绿色：通讯完成
- 橙色（红色+绿色）：通讯失败

![image-20260228092333710](D:\学习笔记\Knowledge-Burger\Picture\stlink\image-20260228092333710.png)

| **仿真器端口** | **连接目标板** | **功能**                  |
| -------------- | -------------- | ------------------------- |
| 1. VDD         | MCU VCC        | 连接STM8目标板的电源VCC   |
| 2. DATA        | MCU SWIM pin   | 连接STM8目标板的SWIM PIN  |
| 3. GND         | GND            | 连接STM8目标板的电源GND   |
| 4. RESET       | MCU RESET pin  | 连接STM8目标板的RESET PIN |

![image-20260228092318935](D:\学习笔记\Knowledge-Burger\Picture\stlink\image-20260228092318935.png)

| **仿真器端口**    | **连接目标板** | **功能**                              |
| ----------------- | -------------- | ------------------------------------- |
| 1. TVCC           | MCU电源VCC     | 连接STM32目标板的电源VCC              |
| 2. TVCC           | MCU电源VCC     | 连接STM32目标板的电源VCC              |
| 3. TRST           | GND            | GROUND                                |
| 4. UART-RX        | GND            | GROUND                                |
| 5. TDI            | TDI            | 连接STM32的JTAG TDI                   |
| 6. UART-TX        | GND            | GROUND                                |
| **7. TMS, SWIO**  | **TMS, SWIO**  | **连接STM32的JTAG的TMS, SWD的SW IO**  |
| 8. BOOT0          | GND            | GROUND                                |
| **9. TCK, SWCLK** | **TCK, SWCLK** | **连接STM32的JTAG的TCK, SWD的SW CLK** |
| 10. SWIM          | GND            | GROUND                                |
| 11. NC            | NC             | Unused                                |
| 12. GND           | GND            | GROUND                                |
| 13. TDO           | TDO            | 连接STM32的JTAG TDO                   |
| 14. SWIM-RST      | GND            | GROUND                                |
| 15. STM32-RESET   | RESET          | 连接STM32目标板的RESET端口            |
| 16. KEY           | NC             | GROUND                                |
| 17. NC            | NC             | Unused                                |
| 18. GND           | GND            | GROUND                                |
| 19. VDD           | NC             | VDD (3.3V)                            |
| 20. GND           | GND            | GROUND                                |

![image-20260228092601391](D:\学习笔记\Knowledge-Burger\Picture\stlink\image-20260228092601391.png)

# 知识补充

**一：JTAG**
JTAG（Joint Test Action Group，联合测试行动小组）是一种国际标准测试协议（IEEE 1149.1兼容），主要用于芯片内部测试。现在多数的高级器件都支持JTAG协议，如ARM、DSP、FPGA器件等。标准的JTAG接口是4线：TMS、 TCK、TDI、TDO，分别为模式选择、时钟、数据输入和数据输出线。相关JTAG引脚的定义为：

| **引脚** | **引脚说明**                                        |
| -------- | --------------------------------------------------- |
| TMS      | 模式选择，TMS用来设置JTAG接口处于某种特定的测试模式 |
| TCK      | 时钟输入                                            |
| TDI      | 数据输入，数据通过TDI引脚输入JTAG接口               |
| TDO      | 数据输出，数据通过TDO引脚从JTAG接口输出             |

**二：SWD**

串行调试（Serial Wire Debug ），是一种和JTAG不同的调试模式，使用的调试协议也不一样，所以最直接的体现在调试接口上，与JTAG的20个引脚相比，SWD只需要4个（或者5个）引脚，结构简单，但是使用范围没有JTAG广泛，主流调试器上也是后来才加的SWD调试模式。

| **引脚** | **引脚说明**                                                 |
| -------- | ------------------------------------------------------------ |
| JTAGV6   | 需要的硬件接口为: GND, RST, SWDIO, SWDCLK                    |
| JTAGV7   | 需要的硬件接口为: GND, RST, SWDIO, SWDCLK，相对V6， 其速度有了明显的提高，速度是 JTAGV6 的 6 倍 |
| JTAGV8   | 需要的硬件接口为: VCC, GND, RST, SWDIO, SWDCLK，速度可以到 10M |

