# JsonSerializerOptions

| 属性                          | 作用             | 典型场景                         |
| ----------------------------- | ---------------- | -------------------------------- |
| `WriteIndented`               | 美化输出         | 调试、日志打印                   |
| `Encoder`                     | 减少字符转义     | 让中文正常显示                   |
| `PropertyNameCaseInsensitive` | 忽略属性名大小写 | 对接第三方接口，字段大小写不统一 |

### **WriteIndented = true**

控制输出是否缩进美化。

- `true`：生成的 JSON 带换行和缩进，方便阅读和调试。
- `false`（默认）：生成紧凑的 JSON，没有多余空格，体积更小。

例如：

```
// WriteIndented = true
{
  "appKey": "xxx",
  "appSecret": "yyy"
}

// WriteIndented = false
{"appKey":"xxx","appSecret":"yyy"}
```

### **Encoder = JavaScriptEncoder.UnsafeRelaxedJsonEscaping**

控制 JSON 中特殊字符如何转义。

默认情况下，`System.Text.Json` 会转义很多非 ASCII 字符，比如中文会变成 `\u4e2d\u6587`。设置成 `UnsafeRelaxedJsonEscaping` 后，会减少转义，让中文、特殊符号等直接输出。

例如：

```
// 默认编码器
{"name":"\u5f20\u4e09"}

// UnsafeRelaxedJsonEscaping
{"name":"张三"}
```

之所以叫 **Unsafe**，是因为减少转义后，如果 JSON 内容包含用户输入的 `<`、`>`、`&` 等字符，直接嵌入 HTML 页面可能有 XSS 风险。但如果是**调用后端接口、内部系统对接**，通常没问题。

### **PropertyNameCaseInsensitive = true**

控制反序列化时属性名是否忽略大小写。

- `true`：`AppKey`、`appkey`、`APPKEY` 都能匹配到 C# 里的 `AppKey` 属性。
- `false`（默认）：属性名必须大小写完全一致。

例如 JSON 是 `{"appkey":"xxx"}`，C# 类属性是 `AppKey`：

```
// PropertyNameCaseInsensitive = true  → 能匹配
// PropertyNameCaseInsensitive = false → 匹配不上，值为 null/default
```