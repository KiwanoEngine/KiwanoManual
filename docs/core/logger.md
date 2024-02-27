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
// [Info] 2020-09-25 16:00:00 Info log
// [Warning] 2020-09-25 16:00:00 Warning log
// [Error] 2020-09-25 16:00:00 Error log
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
// [Info] 2020-09-25 16:00:00  n is 10 , str is str
// [Info] 2020-09-25 16:00:00  f is 1.20
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

#### 日志记录到文件

日志系统提供了 `Provider` 的概念来实现将日志输出到其他地方

```cpp
// 创建一个文件日志 Provider
RefPtr<LogProvider> p = new FileLogProvider("error.log");
// 设置这个 Provider 的日志级别为 Error
p->SetLevel(LogLevel::Error);
// 添加 Provider
Logger::GetInstance().AddProvider(p);
```
