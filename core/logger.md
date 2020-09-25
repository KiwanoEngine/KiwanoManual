### 日志系统

`日志系统Logger` 提供了基于等级的日志记录功能，同时支持格式化日志输出、流式日志输出等多种方式。

#### 日志等级

日志记录分为四个等级，`Level::Info` 信息、`Level::System` 系统、`Level::Warning` 警告和 `Level::Error` 错误，在控制台上显示时通过不同颜色来醒目不同等级的日志。

#### 日志宏

日志系统提供了一系列宏来方便日志记录，分别为 `KGE_LOG`、`KGE_WARN`、`KGE_ERROR`

```cpp
// 输出一条等级为 Info 的日志
KGE_LOG("Info log");
// 输出一条等级为 Warning 的日志
KGE_WARN("Warning log");
// 输出一条等级为 Error 的日志
KGE_ERROR("Error log");

// 输出结果为：
// [INFO] 15:00:00  Info log
// [WARN] 15:00:00  Warning log
// [ERROR] 15:00:00  Error log
```

使用 `KGE_LOG` 和 `KGE_LOGF` 可以实现流式和格式化两种方式的日志记录

```cpp
// 几种类型的变量
int n = 10;
float f = 1.2f;
String str = "str";
// 记录日志（流式）
KGE_LOG("n is", n, ", str is", str);
// 记录日志（格式化）
KGE_LOGF("f is %.2f", f);

// 输出结果为：
// [INFO] 15:00:00  n is 10 , str is str
// [INFO] 15:00:00  f is 1.20
```

#### 单例模式

日志系统使用单例模式，使用 `GetInstance()` 获取该对象的引用。

```cpp
// 获取日志记录器实例
Logger& logger = Logger::GetInstance();
```

#### 开启和关闭

使用 `Enable` 和 `Disable` 方法来开启和关闭日志系统

```cpp
Logger::GetInstance().Enable();
Logger::GetInstance().Disable();
```

#### 重定向输出

日志系统允许将输出重定向到其他流中，例如下面的例子把错误日志和非错误日志分别重定向到不同文件中：

```cpp
// 将非错误日志保存到 log.txt 中
std::ofstream ofs = std::ofstream("log.txt");
// 将错误日志保存到 err.txt 中，且用追加方式保留以前的错误日志
std::ofstream efs = std::ofstream("err.txt", std::ios::app);
// 重定向输出流
if (ofs.is_open())
{
    Logger::GetInstance().RedirectOutputStream(ofs.rdbuf());
}
if (efs.is_open())
{
    Logger::GetInstance().RedirectErrorStream(efs.rdbuf());
}
```
