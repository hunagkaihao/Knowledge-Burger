```
├── build                      // 构建相关  
├── config                     // 配置相关
├── src                        // 源代码
│   ├── api                    // 所有请求
│   ├── assets                 // 主题 字体等静态资源
│   ├── components             // 全局公用组件
│   ├── directive              // 全局指令
│   ├── filtres                // 全局 filter
│   ├── icons                  // 项目所有 svg icons
│   ├── lang                   // 国际化 language
│   ├── mock                   // 项目mock 模拟数据
│   ├── router                 // 路由
│   ├── store                  // 全局 store管理
│   ├── styles                 // 全局样式
│   ├── utils                  // 全局公用方法
│   ├── vendor                 // 公用vendor
│   ├── views                   // view
│   ├── App.vue                // 入口页面
│   ├── main.js                // 入口 加载组件 初始化等
│   └── permission.js          // 权限管理
├── static                     // 第三方不打包资源
│   └── Tinymce                // 富文本
├── .babelrc                   // babel-loader 配置
├── eslintrc.js                // eslint 配置项
├── .gitignore                 // git 忽略项
├── favicon.ico                // favicon图标
├── index.html                 // html模板
└── package.json               // package.json

```

# 基于 Swashbuckle 经典的 WeatherForecast 案例

### **🛠️ 第一步：搭建 Vue 3 项目**

首先，我们需要在本地创建一个 Vue 项目。打开终端（Terminal），运行以下命令：

1. **创建项目**

   ```
   npm create vue@latest
   ```

   - 系统会提示你输入项目名称，例如输入 `vue-weather-client`。
   - 接下来的选项（TypeScript, Router, Pinia 等），为了保持简单，你可以先全部选 **No**（或者根据需求选 Yes，这里我们以最基础的配置为例）。

2. **进入项目并安装依赖**

   ```
   cd vue-weather-client
   npm install
   ```

3. **安装 Axios**
   我们需要安装刚才讲解的 Axios 库来发送请求：

   ```
   npm install axios
   ```

4. **启动项目**

   ```
   npm run dev
   ```

   此时，终端会显示一个本地地址（如 `http://localhost:5173`），按住 `Ctrl` 点击该链接即可在浏览器中打开。

------

### **🎨 第二步：清理默认代码**

Vue 的默认模板包含很多演示代码，我们需要将其清空，只保留最基础的结构。

打开项目中的 `src/App.vue`，将其内容替换为以下最简代码：

```vue
<script setup>
  // 我们将在下一步在这里写逻辑
</script>

<template>
  <div class="container">
    <h1>🌤️ 天气预报查询</h1>
    <p>正在从后端获取数据...</p>
  </div>
</template>

<style scoped>
.container {
  font-family: sans-serif;
  text-align: center;
  margin-top: 50px;
}
</style>
```

------

### **💻 第三步：编写逻辑与后端对接**

假设你的 .NET 后端（Swashbuckle 示例项目）正在 `https://localhost:7001` 运行，并且 WeatherForecast 的接口地址是 `/WeatherForecast`。

我们需要做两件事：

1. **定义数据模型**：告诉 Vue 后端返回的数据长什么样。
2. **调用接口**：使用 Axios 获取数据。

修改 `src/App.vue`，完整代码如下：

```vue
<script setup>
import { ref, onMounted } from 'vue' // 引入 Vue 的核心功能
import axios from 'axios' // 引入 Axios

// 1. 定义响应式数据
// 这对应后端 C# 的 WeatherForecast 类
const weatherList = ref([]) 
const loading = ref(true)
const error = ref(null)

// 2. 定义获取数据的方法
const fetchWeather = async () => {
  try {
    loading.value = true
    error.value = null
    
    // ⚠️ 注意：这里填写你 .NET 后端实际运行的地址
    // 假设后端地址是 https://localhost:7001
    const apiUrl = 'https://localhost:7001/WeatherForecast'
    
    const response = await axios.get(apiUrl)
    
    // 将后端返回的数据赋值给 weatherList
    weatherList.value = response.data
    console.log('获取成功:', response.data)
  } catch (err) {
    error.value = err.message
    console.error('获取失败:', err)
  } finally {
    loading.value = false
  }
}

// 3. 生命周期钩子
// 当组件挂载完成（页面加载）时，自动调用 fetchWeather
onMounted(() => {
  fetchWeather()
})
</script>

<template>
  <div class="container">
    <h1>🌤️ 天气预报查询</h1>
    
    <!-- 加载状态 -->
    <div v-if="loading">正在加载数据...</div>
    
    <!-- 错误状态 -->
    <div v-if="error" class="error">
      出错了: {{ error }}
    </div>
    
    <!-- 数据列表 -->
    <div v-if="!loading && !error">
      <table>
        <thead>
          <tr>
            <th>日期</th>
            <th>温度 (°C)</th>
            <th>摘要</th>
          </tr>
        </thead>
        <tbody>
          <!-- 循环遍历后端返回的数据 -->
          <tr v-for="item in weatherList" :key="item.date">
            <td>{{ item.date }}</td>
            <td>{{ item.temperatureC }}</td>
            <td>{{ item.summary }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.container {
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  max-width: 800px;
  margin: 50px auto;
  text-align: center;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  border-radius: 8px;
}

h1 { color: #42b983; }

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  padding: 12px;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

th { background-color: #f4f4f4; }

.error { color: red; margin-top: 20px; }
</style>
```

------

### **⚠️ 第四步：解决跨域问题 (CORS)**

这是新手最容易遇到的坑。

**现象**：
当你打开浏览器控制台（F12），你会发现请求报错了，提示类似 `Access to XMLHttpRequest ... has been blocked by CORS policy`。

**原因**：
浏览器出于安全考虑，禁止前端页面（运行在 `localhost:5173`）直接请求不同端口的后端（`localhost:7001`）。

**解决方案**：
你需要修改 **.NET 后端代码** 来允许跨域。

1. 在 .NET 项目的 `Program.cs` 中，添加 CORS 服务配置：

```vue
var builder = WebApplication.CreateBuilder(args);

// 1. 添加 CORS 服务
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowVueClient",
        policy => policy.WithOrigins("http://localhost:5173") // 允许前端地址
                        .AllowAnyMethod()
                        .AllowAnyHeader());
});

builder.Services.AddControllers();
// ... 其他配置

var app = builder.Build();

// 2. 使用 CORS 中间件 (必须在 UseAuthorization 之前)
app.UseCors("AllowVueClient"); 

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

1. 重启你的 .NET 后端项目。

------

### **🚀 第五步：最终效果**

1. 确保 .NET 后端正在运行。
2. 确保 Vue 前端正在运行 (`npm run dev`)。
3. 刷新浏览器页面。

你将看到：

1. 页面显示“正在加载数据...”。
2. 约 1 秒后，表格出现，显示后端生成的 5 条天气数据（日期、温度、摘要）。

注意：文件路径不要包含<span style=color:red>**中文**</span>或<span style=color:red>**特殊字符**</span>。