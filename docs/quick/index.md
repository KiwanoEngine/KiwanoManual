### 快速上手

使用下面的代码快速创建一个 Hello World 程序（控制台或Win32应用均可），然后我们解释每一行代码的作用

```cpp
#include <kiwano/kiwano.h>

using namespace kiwano;

void Setup()
{
    // 创建舞台
    RefPtr<Stage> stage = new Stage;

    // 创建一个文本角色
    RefPtr<TextActor> text = new TextActor("Hello World");
    // 设置文字颜色
    text->SetFillColor(Color::White);

    // 将文本添加到舞台中
    stage->AddChild(text);

    // 进入舞台
    Director::GetInstance().EnterStage(stage);
}

#ifdef _CONSOLE
int main()
#else
int WINAPI wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
#endif
{
    // 游戏设置
    Settings s;
    s.window.title = "Hello World"; // 窗口标题
    s.window.width = 640;           // 窗口宽度
    s.window.height = 480;          // 窗口高度

    // 启动应用
    Application::GetInstance().Run(s, Setup);
    return 0;
}
```

### 设置游戏属性

主函数第一行设置了游戏的一些基本属性，调整了游戏窗口标题和窗口大小。

```cpp
Settings s;
// 设置窗口标题
s.window.title = "Hello World";
// 设置窗口大小
s.window.width = 640;
s.window.height = 480;
```

### 启动游戏

接着通过 Application 单例启动游戏，它需要游戏设置 `Settings` 和一个启动函数 `Setup`。

应用启动成功后会自动执行 `Setup` 函数，在这个方法中完成游戏的初始化。

```cpp
Application::GetInstance().Run(s, Setup);
```

### 创建舞台和角色

当游戏启动后，先创建一个舞台Stage，舞台是各种图形、精灵的载体，所有可见物体必须添加到舞台或其子角色中，才会被渲染出来

```cpp
RefPtr<Stage> stage = new Stage;
```

为舞台创建一个文本角色，文字内容为"Hello World"，并设置它的文字颜色为白色

```cpp
RefPtr<TextActor> text = new TextActor("Hello World");
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
