### 运行器

`运行器Runner` 是控制游戏行为的最主要对象，它虽然不能控制游戏生命周期，但可以控制在生命周期内如何完成游戏更新、渲染，并分发窗口事件等。

#### 创建运行器

运行器最主要的功能是执行 `启动函数OnReady` 和 `销毁函数OnDestroy` ，用来初始化游戏和在游戏结束时回收资源。

可以使用继承的方式创建运行器：

```cpp
class MyRunner : public Runner
{
public:
    MyRunner()
    {
        // 运行器设置
        Settings s;
        // ...

        this->SetSettings(s);
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

#### 控制画面帧率

画面帧率（FPS）是指一秒钟内游戏画面的刷新次数，帧率越高画面越流畅，同时也消耗更多的CPU和GPU资源。

Kiwano 默认开启了垂直同步（VSync），也就是说游戏的帧率会尽量等于你的屏幕刷新率，目前大部分屏幕是60Hz，所以你的游戏画面会基本稳定在60 FPS。

如果你需要修改画面帧率，可以首先在运行器设置中取消垂直同步，然后设置帧间隔时间：

```cpp
Settings s;
// 取消垂直同步
s.vsync_enabled = false;
// 设置帧间隔时间
s.frame_interval = 1_sec / 100;
```

`1_sec / 100` 表示 1 秒钟除以 100，也就是每一帧的间隔是 10 毫秒，这样一秒钟就可以刷新 100 帧，即 100 FPS。
