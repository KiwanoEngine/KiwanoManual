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

    bool MainLoop()
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
        return Runner::MainLoop();
    }
};
```
