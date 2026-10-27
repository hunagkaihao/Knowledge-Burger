# nvm（Node Version Manager)

#### **安装 nvm-windows**

- 前往 GitHub 下载最新版 `nvm-setup.exe`：[https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases?spm=5176.28103460.0.0.5a0d2988iFr0v4)
- 安装后重启终端（PowerShell 或 CMD）。

#### **为当前项目安装并切换到 Node.js v16**

```
# 安装 Node.js v16（LTS 版本，兼容 Vue2 + iview）
nvm install 16

# 仅在当前终端会话中切换到 v16（不影响其他项目）
nvm use 16

# 验证版本
node -v  # 应输出 v16.x.x
```

#### **重新安装依赖并启动项目**

```bash
# 删除旧的 node_modules 和 lock 文件
Remove-Item -Recurse -Force node_modules
Remove-Item -Force package-lock.json

# 重新安装依赖
npm install

# 启动开发服务器
npm run serve
```

# 库

## HTTP 客户端库

### Axios

```bash
# 安装
npm install axios

# 导入
import axios from 'axios';

# 基础用法
// GET 请求
const res = await axios.get('/user', { params: { ID: 12345 } });
console.log(res.data); // 真正的响应数据在 res.data 中

// POST 请求
const res = await axios.post('/user', { firstName: 'Fred', lastName: 'Flintstone' });

// 通用配置写法
axios({
  method: 'post',
  url: '/user/12345',
  data: { firstName: 'Fred', lastName: 'Flintstone' },
  timeout: 5000
});

# 创建实例与常用配置
const service = axios.create({
  baseURL: 'https://api.example.com', // 接口基础路径
  timeout: 10000, // 默认超时 10 秒
  headers: { 'X-Custom-Header': 'foobar' } // 自定义请求头
});
export default service;

# 拦截器
// 请求拦截器
service.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// 响应拦截器
service.interceptors.response.use(response => {
  // 直接返回数据部分，业务代码中无需再写 response.data
  return response.data; 
}, error => {
  // 统一处理错误提示
  console.error('请求出错:', error.message);
  return Promise.reject(error);
});

# 请求取消
const controller = new AbortController();

// 发起请求时传入 signal
axios.get('/api/search', { signal: controller.signal })
  .then(res => console.log(res.data))
  .catch(err => {
    if (axios.isCancel(err)) {
      console.log('请求已取消');
    }
  });

// 在需要时（如用户输入新字符或组件卸载）调用 abort 取消请求
controller.abort();

# 并发请求
const [userRes, menuRes] = await Promise.all([
  service.get('/user/info'),
  service.get('/menu/list')
]);

# 错误处理
try {
  await service.get('/some-api');
} catch (error) {
  if (error.response) {
    // 服务器返回了状态码，且状态码不在 2xx 范围内
    console.log(error.response.status);
  } else if (error.request) {
    // 请求已发出，但没有收到响应（如网络断开）
    console.log('网络连接异常');
  } else {
    // 请求配置出错
    console.log(error.message);
  }
}
```

