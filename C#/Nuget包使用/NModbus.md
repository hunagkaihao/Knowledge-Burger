# NModbus 库使用案例

### **🛠️ 第一步：环境准备**

1. 创建项目：
   - 打开终端，执行 `dotnet new console -n ModbusDemo`。
   - 进入目录 `cd ModbusDemo`。
2. 安装库：
   - 执行 `dotnet add package NModbus`。
   - 执行 `dotnet add package Microsoft.Extensions.Hosting` (为了演示标准的 DI 容器)。

------

### **📂 第二步：项目结构规划**

我们将创建三个核心部分：

1. **`IModbusService`**：定义我们要做什么（接口）。
2. **`ModbusService`**：具体怎么做的实现（你的核心逻辑）。
3. **`Program.cs`**：启动入口。

------

### **💻 第三步：编写代码 (重头戏)**

请按照以下步骤逐一创建文件和代码。

#### **1. 创建实体类：响应结果 (DTO)**

首先，我们需要定义通讯的“回执单”。在 `ModbusDemo` 目录下新建文件 `ModbusResponse.cs`：

```c++
// ModbusResponse.cs
namespace ModbusDemo
{
    // 通用响应类
    public class ModbusResponse
    {
        public bool IsSuccess { get; set; }
        public string Message { get; set; } = string.Empty;
        public Exception? Error { get; set; }
    }

    // 读取数据的响应类（继承通用类，多一个Values属性）
    public class ModbusReadResponse : ModbusResponse
    {
        public ushort[] Values { get; set; } = Array.Empty<ushort>();
    }
}
```

#### **2. 定义服务接口**

新建文件 `IModbusService.cs`。这一步是为了规范代码，防止“乱写”。

```c++
// IModbusService.cs
using System.Threading.Tasks;

namespace ModbusDemo
{
    public interface IModbusService
    {
        // 读取保持寄存器
        Task<ModbusReadResponse> ReadHoldingRegistersAsync(ushort startAddress, ushort count);
        // 写入单个寄存器
        Task<ModbusResponse> WriteSingleRegisterAsync(ushort address, ushort value);
    }
}
```

#### **3. 核心实现：搭建你的“通讯大脑”**

这是最核心的部分，请在项目中新建 `ModbusService.cs`。我会在代码中加入详细的注释，解释每一步的物理含义。

```c++
// ModbusService.cs
using System;
using System.Net.Sockets;
using System.Threading.Tasks;
using NModbus;
using NModbus.Device;

namespace ModbusDemo
{
    // 实现接口
    public class ModbusService : IModbusService
    {
        // 1. 定义连接参数（通常从配置文件读取）
        private readonly string _ip = "127.0.0.1"; // 演示用回环地址
        private readonly int _port = 502;          // Modbus TCP 默认端口
        private readonly byte _slaveId = 1;        // 从站ID

        // 2. 核心对象：主站 (Master)
        // 这就是 NModbus 的核心控制器
        private IModbusMaster _master = null!;
        private TcpClient _tcpClient = null!;

        // 构造函数：注入连接信息（模拟从 appsettings.json 读取）
        public ModbusService()
        {
            // 初始化 TCP 客户端
            _tcpClient = new TcpClient();
            // 创建 Modbus 工厂
            var factory = new ModbusFactory();
            // 创建主站对象
            _master = factory.CreateMaster(_tcpClient);
        }

        /// <summary>
        /// 3. 核心步骤：建立连接
        /// 相当于“拿起电话拨号”
        /// </summary>
        private async Task<bool> ConnectAsync()
        {
            try
            {
                // 如果已经连接了，就不重复连
                if (_tcpClient.Connected) return true;

                // ✨ 关键点 1：TCP 三次握手
                // 这行代码就是向 _ip:_port 发送 SYN 包，建立物理通道
                await _tcpClient.ConnectAsync(_ip, _port);
                
                Console.WriteLine($"✅ 物理连接建立成功：{_ip}:{_port}");
                return true;
            }
            catch (Exception ex)
            {
                Console.WriteLine($"❌ 连接失败：{ex.Message}");
                return false;
            }
        }

        /// <summary>
        /// 4. 核心步骤：读取数据 (功能码 0x03 / 0x04)
        /// 相当于“问设备：你现在的状态是什么？”
        /// </summary>
        public async Task<ModbusReadResponse> ReadHoldingRegistersAsync(ushort startAddress, ushort count)
        {
            var response = new ModbusReadResponse();
            
            // 4.1 先确保线路通畅
            if (!await ConnectAsync())
            {
                response.IsSuccess = false;
                response.Message = "无法连接设备";
                return response;
            }

            try
            {
                // ✨ 关键点 2：发送报文 (PDU)
                // 这里发生了什么？
                // 你的电脑组装了一个二进制包，格式大致是：[从站ID][功能码][起始地址 Hi][Lo][数量 Hi][Lo]
                // 然后通过网线发给了服务器。
                // 服务器收到后，从内存里找到对应的数据，再组装成 [从站ID][功能码][字节计数][数据...][CRC校验] 发回来。
                ushort[] registers = await _master.ReadHoldingRegistersAsync(_slaveId, startAddress, count);

                // ✨ 关键点 3：数据解析
                // NModbus 库自动帮你把二进制流解析成了 ushort 数组。
                response.IsSuccess = true;
                response.Values = registers;
                response.Message = $"读取成功，共 {registers.Length} 个寄存器";
                
                Console.WriteLine($"📩 收到数据: [{string.Join(", ", registers)}]");
                return response;
            }
            catch (Exception ex)
            {
                response.IsSuccess = false;
                response.Message = ex.Message;
                response.Error = ex;
                return response;
            }
        }

        /// <summary>
        /// 5. 核心步骤：写入数据 (功能码 0x06)
        /// 相当于“告诉设备：请把速度调到 1500”
        /// </summary>
        public async Task<ModbusResponse> WriteSingleRegisterAsync(ushort address, ushort value)
        {
            var response = new ModbusResponse();
            if (!await ConnectAsync()) 
            { 
                response.Message = "连接失败"; 
                return response; 
            }

            try
            {
                // ✨ 关键点 4：写入报文
                // 组装包：[从站ID][功能码 06][地址][值][CRC]
                // 发送给 PLC。
                await _master.WriteSingleRegisterAsync(_slaveId, address, value);
                
                response.IsSuccess = true;
                response.Message = $"写入成功！地址:{address} -> 值:{value}";
                Console.WriteLine(response.Message);
                return response;
            }
            catch (Exception ex)
            {
                response.IsSuccess = false;
                response.Message = ex.Message;
                return response;
            }
        }

        /// <summary>
        /// 6. 资源释放 (挂电话)
        /// </summary>
        public void Dispose()
        {
            _master?.Dispose();
            _tcpClient?.Close();
            _tcpClient?.Dispose();
            Console.WriteLine("🔌 连接已释放");
        }
    }
}
```

#### **4. 编写启动程序**

修改 `Program.cs`，把刚才写的模块组装起来：

```C++
// Program.cs
using System;
using System.Threading.Tasks;

namespace ModbusDemo
{
    // 这是一个简单的“模拟服务器”类
    // 我们需要一个服务器来接收刚才客户端发的数据
    // 注意：这里使用了 NModbus 内部的模拟功能，不需要你真的去买个 PLC
    /*public class ModbusTcpSlaveSimulator
    {
        private TcpListener _listener;
        private IModbusSlave _slave;

        public ModbusTcpSlaveSimulator(int port, byte slaveId)
        {
            // 1. 创建一个 TCP 监听器（相当于开启了一个网口服务）
            _listener = new TcpListener(System.Net.IPAddress.Any, port);
            
            // 2. 创建一个从站设备 (Slave)
            var factory = new ModbusFactory();
            _slave = factory.CreateSlave(slaveId, null!); // 先不绑定，等会绑定

            // 3. 初始化数据存储区 (Data Store)
            // 这就是设备的“内存”
            var store = _slave.DataStore;
            
            // 初始化 Holding Register 地址 0 的值为 0
            store.HoldingRegisters[0] = 0; 
            // 初始化 Input Register 地址 0 的值为 99 (模拟传感器数据)
            store.InputRegisters[0] = 99; 

            Console.WriteLine("🛠️  模拟设备已启动 (等待客户端连接...)");
        }*/

        // 启动监听
        /*public async Task StartAsync()
        {
            _listener.Start();
            
            while (true)
            {
                // 等待客户端连接
                var client = await _listener.AcceptTcpClientAsync();
                Console.WriteLine("📡 模拟设备：收到客户端连接！");

                // 将从站绑定到客户端
                _slave.NetworkStream = client.GetStream();
                
                // 开始监听报文
                // 这是一个死循环，它会一直解析客户端发来的字节流
                // 如果是读指令，它就从 DataStore 里取数据发回去
                // 如果是写指令，它就把数据存进 DataStore
                await _slave.ListenAsync();
            }
        }
        */
    }

    // --- 主程序入口 ---
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("🧪 开始运行 Modbus 交互演示...\n");
			
            // 开启一个 Slave 服务
            
            // 1. 启动模拟服务器 (模拟硬件设备)
            // 我们在后台开一个线程运行模拟器
            //var simulator = new ModbusTcpSlaveSimulator(502, 1);
            //_ = Task.Run(async () => await simulator.StartAsync());

            // 等待服务器启动（实际项目中不需要这行，这里仅为了演示顺序）
            //await Task.Delay(500);

            // 2. 实例化你的通讯服务 (模拟你的 C# 后端)
            // 注意：这里 IP 写 127.0.0.1，因为我们刚才的模拟器就在本机 502 端口
            var modbusService = new ModbusService();

            try
            {
                // --- 场景演示：读写交互 ---

                // 场景 A：读取设备状态 (Read)
                Console.WriteLine("--- 场景 1：读取 Input Register ---");
                // 我们去读地址 0，读 1 个点
                // 注意：Input Register 的功能码是 04，但在 NModbus 库里通常有专门的方法
                // 为了演示通用性，我们先演示 Holding Register
                var readResult = await modbusService.ReadHoldingRegistersAsync(0, 1);
                if (readResult.IsSuccess)
                {
                    Console.WriteLine($"[Result] 当前寄存器 0 的值是: {readResult.Values[0]} (初始值)\n");
                }

                // 场景 B：下发控制指令 (Write)
                Console.WriteLine("--- 场景 2：写入控制指令 ---");
                var writeResult = await modbusService.WriteSingleRegisterAsync(0, 1234);
                if (writeResult.IsSuccess)
                {
                    Console.WriteLine($"[Action] 已下发指令，将地址 0 的值改为 1234\n");
                }

                // 场景 C：再次读取，验证是否生效
                Console.WriteLine("--- 场景 3：验证写入结果 ---");
                var verifyResult = await modbusService.ReadHoldingRegistersAsync(0, 1);
                if (verifyResult.IsSuccess)
                {
                    Console.WriteLine($"[Verify] 验证成功！当前值已变为: {verifyResult.Values[0]}");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"演示过程中发生异常: {ex.Message}");
            }
            finally
            {
                modbusService.Dispose();
            }

            Console.WriteLine("\n按任意键退出...");
            Console.ReadKey();
        }
    }
}
```

------

### **🚀 第四步：运行与观察**

1. **运行程序**：在终端执行 `dotnet run`。
2. 观察控制台输出：
   - 你会先看到 `模拟设备已启动`。
   - 然后看到 `物理连接建立成功`（这是你的 `ModbusService` 在拨号）。
   - 接着你会看到 `收到数据`（这是模拟器收到了你写的 1234）。
   - 最后验证值确实变了。

### **🧠 核心知识点总结 (基于此 Demo)**

1. **TcpClient 是物理层**：
   - 它是网线。没有它，Modbus 协议跑不了。
   - `ConnectAsync` 就是插上网线的过程。
2. **IModbusMaster 是应用层**：
   - 它是翻译官。你告诉它“我要读地址0”，它负责翻译成 `00 01 00 00 00 06 01 03 00 00 00 01 ...` 这样的二进制流塞进 `TcpClient`。
3. **DataStore 是设备内存**：
   - 在真实的 PLC 里，`HoldingRegisters[0]` 可能对应着真实的电压或电机转速。
   - 在你的 C# 代码里，读写这个数组就是在和现实世界交互。
4. **阻塞与异步 (Async/Await)**：
   - 通讯是耗时的（网络延迟）。如果你用 `ReadSync`（同步），你的整个程序会卡住直到收到数据。
   - 使用 `async/await`，你的程序发完指令就可以去干别的事（比如刷新界面），等数据回来再处理。

### **🔜 下一步建议**

这个 Demo 已经具备了你上传的 `ModbusService.cs` 的核心骨架。

- **进阶练习**：试着在 `ModbusService` 里加一个方法 `ReadTemperature()`，让它去读 `InputRegisters`（通常是只读传感器数据）。
- **对比**：把你这个 Demo 里的 `ReadHoldingRegistersAsync` 方法，和你上传文档里的同名方法对比，你会发现除了多了“长连接管理”和“重试机制”，核心逻辑是一模一样的。