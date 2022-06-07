### 应用程序

`应用程序Application` 控制游戏的整个生命周期，包括游戏初始化、启动、结束以及事件分发等。

#### 单例模式

应用程序是 Kiwano 中最常见的单例模式之一，使用 `GetInstance()` 获取该对象的引用。

```cpp
// 获取应用程序实例
Application& app = Application::GetInstance();
// 调用 Quit() 函数退出游戏
app.Quit();
```

#### 游戏生命周期

应用程序通过 `Run()` 函数启动整个游戏，启动游戏前需要一个 `运行器Runner`：

```cpp
// 创建运行器
RunnerPtr runner = new Runner(window);
// 启动游戏
Application::GetInstance().Run(runner);
```

`Run()` 函数的第二个参数是可选参数，用于指示是否开启调试模式

```cpp
// 在调试状态下启动游戏，第二个参数意为 debug
Application::GetInstance().Run(runner, true);
```

> 调试模式下画面左上角会出现调试信息，用于辅助解决性能问题或内存泄漏等。

在代码的任意地方可以通过 `Quit()` 函数来结束游戏。

```cpp
// 退出游戏
Application::GetInstance().Quit();
```

#### 添加模块

应用程序可以通过添加 `模块Module` 的方式扩展游戏功能，例如Kiwano的音频模块、ImGui模块、HTTP模块都是作为扩展模块添加到游戏中的。

使用 `Use()` 函数添加一个模块：

```cpp
// 获取音频模块实例
audio::AudioModule& am = audio::AudioModule::GetInstance();
// 添加音频模块
Application::GetInstance().Use(am);
```

应用程序会自动完成模块的初始化、运行和销毁，无需关心模块的内部运行过程。

> 注意：添加模块应在 Run 函数启动游戏前执行

#### 事件分发

应用程序控制着整个事件系统，可以使用 `DispatchEvent` 发送用户自定义的事件

```cpp
EventPtr event = ...;
Application::GetInstance().DispatchEvent(event);
```

#### 时间缩放因子

时间缩放因子可以在一定程度上控制游戏速度，例如快进2倍或放慢到0.5倍。

```cpp
// 将游戏时间加快到2倍
Application::GetInstance().SetTimeScale(2.0);
```

> 时间因子只会影响到update与dt相关的部分，也就是仅对 update = f(dt); 的部分生效。  
> 如果用户通过Kiwano以外的方式处理时间，那么时间因子也不生效。

#### 在主线程中执行函数

如果你在多线程下工作，在其他线程的代码下需要执行一段主线程运行的函数时，可以通过 `PerformInMainThread` 函数执行

```cpp
void TestFunc()
{
    KGE_LOG("Run test!");
}

// 在主线程中执行 TestFunc 函数
Application::GetInstance().PerformInMainThread(TestFunc);
```

> PerformInMainThread() 是非阻塞的，执行完后立即返回，并在主线程执行回调函数时阻塞
