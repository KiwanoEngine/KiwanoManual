### 窗口

`窗口Window` 控制了应用程序窗口的大小、标题等属性。

#### 创建窗口

窗口是由[运行器](./runner.md#运行器选项)自动创建的，窗口的属性在运行器选项中指定：

```cpp
// 运行器选项
Settings s;
// 设置窗口标题
s.window.title = "Hello World";
// 设置窗口大小
s.window.width = 640;
s.window.height = 480;
```

窗口设置的完整定义为

```cpp
struct WindowConfig
{
    uint32_t width  = 640;            ///< 窗口宽度
    uint32_t height = 480;            ///< 窗口高度
    String   title  = "Kiwano Game";  ///< 窗口标题
    Icon     icon;                    ///< 窗口图标
    bool     resizable  = false;      ///< 窗口大小可调整
    bool     fullscreen = false;      ///< 窗口全屏
};
```

#### 窗口图标

在Visual Studio中将ico类型的图标资源添加到程序中，然后使用资源ID设置图标

```cpp
// 假设 ico 图片导入后的资源ID为 IDI_ICON1
Settings s;
s.window.icon = Icon(IDI_ICON1);
```

#### 获取主窗口

在代码的任意位置，可以通过 `应用程序Application` 获取主窗口

```cpp
RefPtr<Window> window = Application::GetInstance().GetWindow();
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
