### 加载HTTP模块

引入头文件

```cpp
#include <kiwano-network/kiwano-network.h>
using namespace kiwano::network;
```

在游戏启动时引入网络模块

```cpp
Application::GetInstance().Run(settings, Setup, { HttpModule::GetInstancePtr() });
```

如果是使用自定义 Runner 的方式启动，则可以直接在 Runner 的构造函数中引入模块

```cpp
class MyRunner : public Runner
{
public:
    MyRunner()
    {
        Application::GetInstance().Use(HttpModule::GetInstance());
    }
};
```

### 发起GET请求

```cpp
// 创建一个HTTP GET请求
HttpRequestPtr request = new HttpRequest(
    "http://httpbin.org/get",
    HttpType::Get,
    [](HttpRequestPtr req, HttpResponsePtr resp) {
        // 收到响应后打印内容
        KGE_LOG(resp->GetData());
    }
);

// 发送 HTTP 请求
HttpModule::GetInstance().Send(request);
```

### 发起POST请求

```cpp
// 创建 JSON 格式的 POST 数据
Json request_data = {
    { "string", "a simple string" },
    { "boolean", true },
    { "integer", 12 },
    { "float", 3.125 },
    { "array", { 1, 2, 3, 4, 4.5 } },
    { "object", {{ "key", "value" }, { "key2", "value2" }} },
};

// 处理响应
auto callback = [](HttpRequestPtr req, HttpResponsePtr resp)
{
    // 判断请求是否成功
    if (resp->IsSucceed())
    {
        try
        {
            // 将获取到的数据解析成 JSON 格式
            Json response_data = Json::parse(resp->GetData());
            Json result = {
                {"HttpCode", resp->GetResponseCode()},
                {"Data", response_data},
            };

            // 打印结果
            KGE_LOG(result);
        }
        catch (std::exception& e)
        {
            // 解析json数据失败时，打印错误
            KGE_ERROR("Parse JSON failed! ", e.what());
        }
    }
    else
    {
        // 请求失败时，打印 HTTP 响应结果的状态码和错误信息
		Json result = {
				{"HttpCode", response->GetResponseCode()},
				{"Error", response->GetError()},
		};
        KGE_ERROR(result);
    }
};

// 创建一个HTTP POST请求
HttpRequestPtr request = new HttpRequest("http://httpbin.org/post", HttpType::Post, request_data, callback);

// 发送 HTTP 请求
HttpModule::GetInstance().Send(request);
```
