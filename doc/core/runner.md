### 运行器

`运行器Runner` 是控制游戏行为的最主要对象，它虽然不能控制游戏生命周期，但可以控制在生命周期内如何完成游戏更新、渲染，并分发窗口事件等。

#### 创建运行器

运行器最主要的功能是执行 `启动函数Setup` 和 `销毁函数Destroy` ，用来初始化游戏和在游戏结束时回收资源。

可以使用回调函数创建一个运行器：

```cpp
// 初始化回调函数
void Setup()
{
    // 完成游戏初始化
}

// 销毁回调函数
void Destroy()
{
    // 完成游戏资源回收
}

// 创建运行器
RunnerPtr runner = Runner::Create(window, Setup, Destroy);
```

也可以使用继承的方式创建运行器：

```cpp
class MyRunner : public Runner
{
public:
    MyRunner()
    {
        // 创建窗口
        WindowPtr window = ...;
        // 设置主窗口
        this->SetMainWindow(window);
    }

    void OnReady()
    {
        // 完成游戏初始化
    }

    void OnDestroy()
    {
        // 完成游戏资源回收
    }
};

// 创建运行器
RunnerPtr runner = new MyRunner;
```

#### 处理窗口关闭

当用户点击窗口的关闭按钮时，可以通过继承方式给用户弹出一个提示窗，询问用户是否关闭：

```cpp
class MyRunner : public Runner
{
public:
    bool OnClosing()
    {
        int ret = MessageBox(NULL, TEXT("确认要退出吗?"), TEXT("退出"), MB_YESNO | MB_ICONQUESTION);
        if (ret == IDYES)
        {
            // 返回 true 表示允许用户退出
            return true;
        }
        return false;
    }
};
```

#### 控制主循环

如果你需要在游戏主循环上添加代码，可以通过继承的方式控制主循环：

```cpp
class MyRunner : public Runner
{
public:
    int x = 0;

    bool MainLoop(Duration dt)
    {
        // 执行 100 次主循环后退出游戏
        x++;
        KGE_LOG("当前循环次数：", x);

        if (x == 100)
        {
            // 返回 false 表示结束主循环
            return false;
        }
        // 否则执行默认主循环
        return Runner::MainLoop(dt);
    }
};
```

通过重载MainLoop函数，可以实现自定义的画面帧数：

```cpp
#include <timeapi.h>  // timeBeginPeriod, timeEndPeriod
#pragma comment(lib, "winmm.lib")

class MyRunner
    : public Runner
{
public:
    MyRunner()
    {
        // 关闭垂直同步
        Renderer::GetInstance().SetVSyncEnabled(false);
    }

    bool MainLoop(Duration dt)
    {
        // 画面刷新的时间间隔，一秒刷新 100 次
        static const Duration interval = 1_sec / 15;
        // 时间增量
        static Duration deltaTime;

        // 加上上一次执行主循环到现在的时间间隔
        deltaTime += dt;
        if (deltaTime >= interval)
        {
            // 时间增量大于画面刷新的时间间隔时，执行主循环
            bool result = Runner::MainLoop(deltaTime);
            deltaTime = 0;
            return result;
        }
        else
        {
            // 未到达时间间隔时调用 Sleep 释放 CPU
            long ms = (interval - deltaTime).Milliseconds();
            if (ms > 1)
            {
                ::Sleep(ms);
            }
            return true;
        }
    }

    void OnReady()
    {
        // 提高 Sleep 时间精度，防止 Sleep 时间不稳定
        ::timeBeginPeriod(1);
    }

    void OnDestroy()
    {
        // 还原时间精度
        ::timeEndPeriod(1);
    }
};
```

从更专业的角度来说，上面的代码没有考虑到画面间隔的误差问题。

例如，虽然期望时间间隔是16毫秒一帧，但是由于Sleep函数误差或其他原因，导致每次画面刷新的时间间隔总是比16毫秒大，最终呈现出的画面就不是1秒60帧而有可能是1秒50帧。

下面的代码是考虑了时间误差并尝试修复的代码：

```cpp
class MyRunner
    : public Runner
{
public:
    bool MainLoop(Duration dt)
    {
        // 画面刷新的时间间隔，一秒刷新 100 次
        static const Duration interval = 1_sec / 100;
        // 不包含误差的时间增量
        static Duration deltaTime;
        // 时间误差
        static Duration errorTime;

        // 加上上一次执行主循环到现在的时间间隔
        deltaTime += dt;
        // 计算包含误差的时间增量
        Duration totalDt = deltaTime + errorTime;
        if (totalDt >= interval)
        {
            // 时间增量大于画面刷新的时间间隔时，执行主循环
            bool result = Runner::MainLoop(deltaTime);

            // 记录误差
            errorTime = totalDt - interval;
            deltaTime = 0;
            return result;
        }
        else
        {
            // 未到达时间间隔时调用 Sleep 释放 CPU
            long ms = (interval - totalDt).Milliseconds();
            if (ms > 1)
            {
                ::Sleep(ms);
            }
            return true;
        }
    }
};
```
