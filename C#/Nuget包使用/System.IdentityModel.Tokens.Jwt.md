# System.IdentityModel.Tokens.Jwt

### **📚 第一部分：核心概念图解**

在写代码前，先要在脑海里建立模型：

1. JWT (JSON Web Token)：
   - 把它想象成**游乐场的“手环”**。
   - 你在门口（登录接口）出示身份证（账号密码）。
   - 工作人员核实后，给你一个加密的手环（Token）。
   - 玩项目（访问 API）时，不需要再出示身份证，只要亮出手环就行。
2. 认证 vs 授权：
   - **认证**：检票员看你的手环是不是真的（签名验证），是不是过期的。
   - **授权**：检票员看你的手环是“VIP”还是“普通”，决定你能不能进 VIP 厅。
3. 中间件：
   - 就是那个**检票员**。在请求到达你的 `Controller`（游乐设施）之前，中间件会先拦截请求，检查 Header 里有没有 Token。

------

### **🛠️ 第二部分：代码实现 (三步走)**

#### **步骤 1：准备“制证中心” (JwtUtils)**

我们需要一个工具类来负责生成和验证 Token。

1. **安装 NuGet 包**：
   在终端运行：

   ```
   dotnet add package System.IdentityModel.Tokens.Jwt
   ```

2. **创建 `JwtUtils.cs`**：
   在项目中创建一个新文件夹 `Helpers` 或 `Utils`，添加以下代码：

```C#
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;
using Microsoft.IdentityModel.Tokens;

namespace YourNamespace.Helpers
{
    public class JwtUtils
    {
        // 密钥（在生产环境中，这应该放在 appsettings.json 中，且非常复杂）
        // 注意：长度至少要 16 个字符，否则报错
        private readonly string _secretKey = "ThisIsASuperSecretKeyForDemo123!"; 

        // 生成 Token
        public string GenerateToken(string username, string role)
        {
            // 1. 准备 Token 的内容（声明）
            var claims = new[]
            {
                new Claim(ClaimTypes.Name, username), // 用户名
                new Claim(ClaimTypes.Role, role)      // 角色（用于授权）
            };

            // 2. 准备密钥（对称密钥）
            var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_secretKey));
            var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

            // 3. 创建 Token
            var token = new JwtSecurityToken(
                issuer: "MyAgvSystem",          // 发行人
                audience: "MyAgvClient",        // 接收人
                claims: claims,                 // 用户信息
                expires: DateTime.Now.AddHours(2), // 过期时间（2小时）
                signingCredentials: creds       // 签名凭证
            );

            // 4. 将 Token 对象转换为字符串
            return new JwtSecurityTokenHandler().WriteToken(token);
        }
    }
}
```

#### **步骤 2：建立“安检通道” (配置中间件)**

我们需要告诉 .NET 程序：“嘿，以后所有的请求都要检查 Token”。

1. **修改 `Program.cs`**：

```C#
using System.Text;
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;

var builder = WebApplication.CreateBuilder(args);

// --- 1. 注册服务 ---
builder.Services.AddControllers();
builder.Services.AddScoped<JwtUtils>(); // 注册我们的制证工具

// --- 2. 配置认证服务 (安检规则) ---
var secretKey = "ThisIsASuperSecretKeyForDemo123!"; // 必须和 JwtUtils 里的一致
var key = Encoding.UTF8.GetBytes(secretKey);

builder.Services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidateLifetime = true, // 验证过期时间
        ValidateIssuerSigningKey = true, // 验证签名
        ValidIssuer = "MyAgvSystem",
        ValidAudience = "MyAgvClient",
        IssuerSigningKey = new SymmetricSecurityKey(key)
    };
});

// --- 3. 构建应用 ---
var app = builder.Build();

// --- 4. 使用中间件 (关键顺序) ---
app.UseHttpsRedirection();
app.UseRouting();

// ⚠️ 注意顺序：必须先 UseAuthentication，再 UseAuthorization
app.UseAuthentication(); 
app.UseAuthorization();

app.MapControllers();

app.Run();
```

#### **步骤 3：实战演练 (登录与保护)**

1. **创建/修改 `AuthController` (登录接口)**：

```c#
[Route("api/[controller]")]
[ApiController]
public class AuthController : ControllerBase
{
    private readonly JwtUtils _jwtUtils;

    public AuthController(JwtUtils jwtUtils)
    {
        _jwtUtils = jwtUtils;
    }

    [HttpPost("login")]
    public IActionResult Login([FromBody] LoginRequest request)
    {
        // 1. 模拟验证账号密码 (实际应从数据库查)
        // 假设账号是 admin，密码是 123456
        if (request.Username == "admin" && request.Password == "123456")
        {
            // 2. 生成 Token (假设 admin 是管理员角色)
            var token = _jwtUtils.GenerateToken(request.Username, "Admin");
            return Ok(new { Token = token, Message = "登录成功" });
        }

        return Unauthorized("用户名或密码错误");
    }
}

// 辅助类
public class LoginRequest
{
    public string Username { get; set; }
    public string Password { get; set; }
}
```

1. **保护 `AgvStatusController` (授权)**：

在你的 Modbus 或 AGV 控制器上加上 `[Authorize]` 标签。

```c#
[ApiController]
[Route("api/[controller]")]
[Authorize] // 🔒 加上这一行！任何请求都必须带有效 Token
    public class WeatherForecastController : ControllerBase
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        private readonly ILogger<WeatherForecastController> _logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger)
        {
            _logger = logger;
        }

        [HttpGet(Name = "GetWeatherForecast")]
        public IEnumerable<WeatherForecast> Get(int count = 5)
        {
            return Enumerable.Range(1, count).Select(index => new WeatherForecast
            {
                Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                TemperatureC = Random.Shared.Next(-20, 55),
                Summary = Summaries[Random.Shared.Next(Summaries.Length)]
            })
            .ToArray();
        }
    }
```

------

### **🧪 第三部分：如何测试**

这是验证你学习成果的关键步骤。

1. **测试未授权访问**：

   - 运行程序，在 Swagger UI 中 发送 `GET` 请求到 `/api/WeatherForecast`。
   - **预期结果**：返回 **401 Unauthorized**。这说明安检员（中间件）工作了，把你拦在了门外。

2. **获取 Token**：

   - 发送 `POST` 请求到 `/api/Auth/login`。

   - Body (JSON):

     ```
     {
       "username": "admin",
       "password": "123456"
     }
     ```

   - **预期结果**：返回 **200 OK**，并且 Response 里有一串很长的字符串（Token）。

3. **测试授权访问**：

   - 请在 `Program.cs` 中，找到 `builder.Services.AddSwaggerGen()` 这一段，加入以下代码：

   ```c#
   builder.Services.AddSwaggerGen(c =>
   {
       // ... 你原有的代码 ...
   
       // 1. 定义安全方案 (告诉 Swagger 我们要用 JWT)
       c.AddSecurityDefinition("Bearer", new Microsoft.OpenApi.Models.OpenApiSecurityScheme
       {
           Description = "JWT Authorization header using the Bearer scheme. Example: \"Authorization: Bearer {token}\"",
           Name = "Authorization", // 请求头名称
           In = Microsoft.OpenApi.Models.ParameterLocation.Header, // 在 Header 中传递
           Type = Microsoft.OpenApi.Models.SecuritySchemeType.ApiKey, // 类型为 ApiKey
           Scheme = "Bearer"
       });
   
       // 2. 定义安全要求 (全局启用，或者你可以只在需要的 Controller 上启用)
       c.AddSecurityRequirement(new Microsoft.OpenApi.Models.OpenApiSecurityRequirement()
       {
           {
               new Microsoft.OpenApi.Models.OpenApiSecurityScheme
               {
                   Reference = new Microsoft.OpenApi.Models.OpenApiReference
                   {
                       Type = Microsoft.OpenApi.Models.ReferenceType.SecurityScheme,
                       Id = "Bearer"
                   },
                   Scheme = "oauth2",
                   Name = "Bearer",
                   In = Microsoft.OpenApi.Models.ParameterLocation.Header,
               },
               new List<string>()
           }
       });
   });

​		修改完代码并重启程序后，刷新 Swagger 页面，你会看到变化：

1. **点击右上角的 "Authorize" 按钮**。

2. 会弹出一个输入框，里面写着 `Bearer`。

3. **输入格式**：你需要把图 1 中获取到的长字符串复制下来，并在前面加上 `Bearer `（注意 Bearer 后面有一个空格）。

   > **格式示例：**
   > `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

4. 点击 "Authorize"，然后关闭弹窗。

5. 现在，再去点击你的 `AgvStatus` 接口（图 2 那个接口）并执行，你会发现 **401 错误消失了，返回了 200 OK**！