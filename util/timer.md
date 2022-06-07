### 计时器

计时器，或者叫秒表可能更贴切一点。

Usage:

```cpp
// 创建计时器，此时时间记为 t1
TimerPtr timer = new Timer;

// 做点什么耗时的工作

// 按一下秒表计时，此时时间记为 t2
timer->Tick();

// 可以获取 t1 ~ t2 的时间间隔
Duration d1_2 = timer->GetDeltaTime();

// 再做点什么耗时的工作

// 按一下秒表计时，此时时间记为 t3
timer->Tick();

// 可以获取 t2 ~ t3 的时间间隔
Duration d2_3 = timer->GetDeltaTime();

// 也可以获取 t1 ~ t3 的总时长
Duration d1_3 = timer->GetTotalTime();

// 除此之外还有暂停、重置功能
timer->Pause();  // 暂停计时
timer->Resume(); // 继续计时
timer->Reset();  // 计时清零
```
