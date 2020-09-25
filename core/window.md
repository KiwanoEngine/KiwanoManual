### 窗口

`窗口Window` 控制了应用程序窗口的大小、标题等属性。

#### 创建窗口

指定窗口的标题和大小来创建一个简单窗口：

```cpp
// 创建一个标题为 "A Simple Window"，大小为 600x400 的窗口
WindowPtr window = new Window("A Simple Window", 600, 400);
```

还可以指定窗口的图标，在Visual Studio中将ico类型的图标资源添加到程序中，然后使用资源ID设置图标

```cpp
// IDI_ICON1 是VS中的图标资源ID
WindowPtr window = new Window("A Simple Window", 600, 400, IDI_ICON1);
```

`Window::Create` 函数的完整定义为：

```cpp
WindowPtr Create(
    const String& title,    // 窗口标题
    uint32_t width,         // 窗口宽度
    uint32_t height,        // 窗口高度
    uint32_t icon = 0,      // 窗口图标
    bool resizable = false, // 是否大小可变
    bool fullscreen = false // 是否全屏
);
```

#### 获取主窗口

在代码的任意位置，可以通过 `应用程序Application` 获取主窗口

```cpp
WindowPtr window = Application::GetInstance().GetMainWindow();
```

#### 常用方法

- 获取窗口属性

```cpp
// 获取窗口标题
String title = window->GetTitle();
// 获取窗口宽度
uint32_t width = window->GetWidth();
// 获取窗口高度
uint32_t height = window->GetHeight();
// 获取窗口大小
Size size = window->GetSize();
// 获取窗口句柄，在 win32 下 WindowHandle 为 HWND 类型
WindowHandle handle = window->GetHandle();
```

- 修改窗口属性

```cpp
// 修改窗口标题
window->GetTitle("New Title");
// 修改窗口大小
window->Resize(600, 400);
// 修改鼠标类型为小手
window->SetCursor(CursorType::Hand);
```
