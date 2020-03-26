### 快速上手

使用下面的代码快速创建一个 Hello World 程序，然后我们解释每一行代码的作用

```cpp
#include <kiwano/kiwano.h>

using namespace kiwano;

// 启动函数
void Startup();

int main()
{
    // 创建窗口
    WindowPtr window = Window::Create("Hello World", 640, 480);
    // 创建运行器
    RunnerPtr runner = Runner::Create(window, Startup);
    // 运行
    Application::GetInstance().Run(runner);
    return 0;
}

void Startup()
{
    // 创建舞台
    StagePtr stage = Stage::Create();
    // 创建文本角色
    TextActorPtr text = TextActor::Create("Hello World");
    // 设置文字颜色
    text->SetFillColor(Color::White);
    // 将文本添加到舞台中
    stage->AddChild(text);
    // 进入舞台
    Director::GetInstance().EnterStage(stage);
}
```

主函数第一行创建了一个标题为 "Hello World"、大小为 640 x 480 的窗口

```cpp
WindowPtr window = Window::Create("Hello World", 640, 480);
```

然后为此窗口创建一个运行器，运行器需要一个`启动函数`，启动函数是 Kiwano 完成初始化后自动运行的函数，这个函数是游戏逻辑的入口

```cpp
RunnerPtr runner = Runner::Create(window, Startup);
```

接着通过 Application 单例启动刚刚创建的运行器，应用程序初始化完成后会执行启动函数 `Startup`

```cpp
Application::GetInstance().Run(runner);
```

当游戏启动后，先创建一个舞台Stage，舞台是各种图形、精灵的载体，所有可见物体必须添加到舞台或其子角色中，才会被渲染出来

```cpp
StagePtr stage = Stage::Create();
```

为舞台创建一个文本角色，文字内容为"Hello World"，并设置它的文字颜色为白色

```cpp
TextActorPtr text = TextActor::Create("Hello World");
text->SetFillColor(Color::White);
```

将文本角色添加到舞台中，否则这个角色不会显示在画面上，添加角色使用舞台的 `AddChild` 方法

```cpp
stage->AddChild(text);
```

通过 Director 导演进入舞台，游戏画面上会显示出刚刚创建的文字

```cpp
Director::GetInstance().EnterStage(stage);
```
