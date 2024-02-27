### 加载UI模块

引入头文件

```cpp
#include <kiwano-imgui/kiwano-imgui.h>
using namespace kiwano;
```

在游戏启动时引入UI模块

```cpp
Application::GetInstance().Run(settings, Setup, { imgui::Module::GetInstancePtr() });
```

如果是使用自定义 Runner 的方式启动，则可以直接在 Runner 的构造函数中引入模块

```cpp
class MyRunner : public Runner
{
public:
    MyRunner()
    {
        Application::GetInstance().Use(imgui::Module::GetInstance());
    }
};
```
