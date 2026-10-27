# 示例1

```C#
private HttpClient CreateU8Client() => _httpClientFactory?.CreateClient("U8") ?? new HttpClient();
||
||等价于
\/
private HttpClient CreateU8Client()
{
	if(_httpClientFactory == null)
    {
        return null;
    }
    var client = _httpClientFactory.CreateClient("U8");
	if (client != null)
    {
        retrun client;
    }
	return new HttpClient();
}
```

