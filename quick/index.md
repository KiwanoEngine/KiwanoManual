### 快速上手

使用下面的代码快速创建一个 Hello World 程序，然后我们解释每一行代码的作用

```cpp
#include <kiwano/kiwano.h>

using namespace kiwano;

class MyRunner : public Runner
{
public:
    MyRunner()
    {
        // 运行器选项
        Settings s;
        // 窗口标题设为 Hello World，大小设置为 640x480
        s.window.title = "Hello World";
        s.window.width = 640;
        s.window.height = 480;

        this->SetSettings(s);
    }

    void OnReady() override
    {
        // 创建舞台
        StagePtr stage = new Stage;

        // 创建一个文本角色
        TextActorPtr text = new TextActor("Hello World");
        // 设置文字颜色
        text->SetFillColor(Color::White);

        // 将文本添加到舞台中
        stage->AddChild(text);

        // 进入舞台
        Director::GetInstance().EnterStage(stage);
    }
};

#ifdef _CONSOLE
int main()
#else
int WINAPI wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
#endif
{
    // 创建运行器
    RunnerPtr runner = new MyRunner;

    // 启动运行器
    Application::GetInstance().Run(runner);
    return 0;
}
```

### 创建运行器

主函数第一行创建了一个运行器，它可以控制游戏开始和结束

```cpp
RunnerPtr runner = new MyRunner;
```

### 创建窗口

运行器的构造函数中设置了游戏窗口的属性，创建了一个标题为 "Hello World"、大小为 640 x 480 的窗口

```cpp
Settings s;
// 设置窗口标题
s.window.title = "Hello World";
// 设置窗口大小
s.window.width = 640;
s.window.height = 480;
```

### 启动游戏

接着通过 Application 单例启动刚刚创建的运行器

```cpp
Application::GetInstance().Run(runner);
```

运行器启动成功后会自动执行它的 `OnReady()` 方法，在这个方法中完成游戏的初始化

### 创建舞台和角色

当游戏启动后，先创建一个舞台Stage，舞台是各种图形、精灵的载体，所有可见物体必须添加到舞台或其子角色中，才会被渲染出来

```cpp
StagePtr stage = new Stage;
```

为舞台创建一个文本角色，文字内容为"Hello World"，并设置它的文字颜色为白色

```cpp
TextActorPtr text = new TextActor("Hello World");
// 设置文字的填充颜色
text->SetFillColor(Color::White);
```

将文本角色添加到舞台中，否则这个角色不会显示在画面上，添加角色使用舞台的 `AddChild` 方法

```cpp
stage->AddChild(text);
```

### 进入舞台

通过 Director 导演进入舞台，游戏画面上会显示出刚刚创建的文字

```cpp
Director::GetInstance().EnterStage(stage);
```
