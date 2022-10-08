### 时钟系统

时钟系统由 `时间Time`、`时钟时间ClockTime`、`时间段Duration` 三个类组成。

#### 时间段

游戏中很多地方需要使用时间段，例如动画的持续时长、定时任务的间隔时间等等，可以用下面的方法在代码中创建一个时间段

```cpp
// 50 毫秒
Duration d = time::Millisecond * 50;
// 5 秒
Duration d = time::Second * 5;
// 1.5 小时
Duration d = time::Hour * 1.5;
// 3 小时 45 分 15秒
Duration d = time::Hour * 3 + time::Minute * 45 + time::Second * 15;
```

在 VS2015 及更高版本可以使用 literals 来表示时间：

```cpp
using namespace kiwano;
Duration d = 50_msec;                   // 50 毫秒
Duration d = 5_sec;                     // 5 秒
Duration d = 1.5_hour;                  // 1.5 小时
Duration d = 3_hour + 45_min + 15_sec;  // 3 小时 45 分 15 秒
```

时间段可以做加减乘除等操作

```cpp
// 1小时减15分钟，得到45分钟
Duration min45 = 1_hour - 15_min;
// 15分钟的2倍是30分钟
Duration min30 = 15_min * 2;
```

可以把时间段转换为毫秒、秒、分钟和小时

```cpp
Duration d = 15_min;
// 转换为毫秒
int64_t ms = d.GetMilliseconds();
// 转换为秒
float s = d.GetSeconds();
// 转换为分钟
float m = d.GetMinutes();
// 转换为小时
float h = d.GetHours();
```

时间段可以转化为字符串，同时也可以和输出流交互

```cpp
// 3 小时 45 分 15 秒
Duration d = 3_hour + 45_min + 15_sec;
// 转化为字符串
String s = d.ToString();  // 得到：3h45m15s
// 直接输出
std::cout << d;  // 输出：3h45m15s
```

同样也支持从字符串解析时间段

```cpp
Duration d = Duration::Parse("3h45m15s");
```

#### 时间

获取当前时间：

```cpp
Time now = Time::Now();
```

时间可以做加减法

```cpp
Time t1 = Time::Now();
// ...
// 做点什么
Time t2 = Time::Now();
// 计算两个时间点之间的时间间隔
Duration t = t2 - t1;
```

> 注意：Time是与系统时间无关的，用户修改系统时间并不会影响Time类的准确性，但也同时导致了Time无法获取年月日等信息，也无法转换为时间戳

#### 时钟时间

获取系统的时钟之间：

```cpp
ClockTime now = ClockTime::Now();
```

时钟时间同样可以做加减法

```cpp
ClockTime t1 = ClockTime::Now();
// ...
// 做点什么
ClockTime t2 = ClockTime::Now();
// 计算两个时间点之间的时间间隔
Duration t = t2 - t1;
```

获取时间戳

```cpp
int64_t unix = ClockTime::Now().GetTimeStamp();
```

从时间戳转换为时间

```cpp
int64_t unix = 1609430400;
ClockTime t = ClockTime::FromTimeStamp(unix);
```

把时钟时间转换为C风格的时间

```cpp
time_t c_time = ClockTime::Now().GetCTime();
```
