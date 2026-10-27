# StringContent

`StringContent` 是 .NET 中 `System.Net.Http` 命名空间下的一个类，用来把**字符串包装成 HTTP 请求体内容**。

它继承自 `ByteArrayContent`，而 `ByteArrayContent` 又继承自 `HttpContent`，所以它本质上是 **HTTP 请求体的一种载体**。

```
var getContent = new StringContent(
    pushDataJson,              // 请求体字符串，通常是 JSON
    Encoding.UTF8,             // 编码方式
    "application/json"         // 媒体类型 / Content-Type
);
```

这行代码的意思是：把 `pushDataJson` 这个 JSON 字符串，按 UTF-8 编码包装成一个 HTTP 内容对象，并告诉服务端：**请求体的格式是 JSON**。

| 参数        | 含义                        | 示例                     |
| ----------- | --------------------------- | ------------------------ |
| `content`   | 请求体字符串                | `"{\"appKey\":\"xxx\"}"` |
| `encoding`  | 字符编码                    | `Encoding.UTF8`          |
| `mediaType` | 媒体类型，即 `Content-Type` | `"application/json"`     |

**常见用法**

通常配合 `HttpClient` 的 `PostAsync`、`PutAsync`、`SendAsync` 使用：

```C#
using var client = new HttpClient();

var pushDataJson = JsonSerializer.Serialize(loginData);

var content = new StringContent(
    pushDataJson,
    Encoding.UTF8,
    "application/json"
);

var response = await client.PostAsync("https://u8.example.com/api/push", content);
```

如果不传第三个参数，默认媒体类型是 `text/plain`。 所以调用 JSON 接口时，一定要显式写成 `"application/json"`，否则服务端可能无法正确识别请求体格式。