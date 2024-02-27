### 加载音频模块

引入头文件

```cpp
#include <kiwano-audio/kiwano-audio.h>
using namespace kiwano;
```

在游戏启动时引入音频模块

```cpp
Application::GetInstance().Run(settings, Setup, { audio::Module::GetInstancePtr() });
```

如果是使用自定义 Runner 的方式启动，则可以直接在 Runner 的构造函数中引入模块

```cpp
class MyRunner : public Runner
{
public:
    MyRunner()
    {
        Application::GetInstance().Use(audio::Module::GetInstance());
    }
};
```
