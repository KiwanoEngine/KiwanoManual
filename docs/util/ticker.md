### 报时器

报时器在计时器的基础上增加了判断时间间隔的功能

```cpp
// 创建一个每1秒触发一次，总共触发5次的报时器
RefPtr<Ticker> ticker = new Ticker(1_sec, 5);

// ticker->Tick() 函数会在达到触发时间时返回true

for (;;)
{
    // 判断是否达到触发条件
    if (ticker->Tick())
    {
        KGE_LOG("tick!");
        // 判断触发次数是否达到最大
        if (ticker->GetTickedCount() == ticker->GetTotalTickCount())
        {
            KGE_LOG("done!");
            break;
        }
    }
    // 等待10毫秒
    (10_msec).Sleep();
}
```

你可以在控制台中看到这样的输出：

```
[Info] 2022-01-01 12:00:00 tick!
[Info] 2022-01-01 12:00:01 tick!
[Info] 2022-01-01 12:00:02 tick!
[Info] 2022-01-01 12:00:03 tick!
[Info] 2022-01-01 12:00:04 tick!
[Info] 2022-01-01 12:00:04 done!
```
