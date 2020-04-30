### 渲染器

`渲染器Renderer` 为用户提供了渲染相关的控制方法，例如。

#### 单例模式

渲染器使用单例模式，使用 `GetInstance()` 获取该对象的引用。

```cpp
// 获取渲染器实例
Renderer& renderer = Renderer::GetInstance();
```

#### 设置清屏颜色

清屏颜色是窗口在未渲染时的背景色，通过 `SetClearColor` 方法来设置清屏色

```cpp
// 清屏颜色设为白色
Renderer::GetInstance().SetClearColor(Color::White);
```

#### 设置垂直同步

垂直同步是指游戏画面的刷新次数与显示器刷新频率同步，例如显示器的刷新频率为 60Hz，那么游戏画面的帧数（FPS）也基本维持在60帧左右。

如果追求更高的画面帧数，或者由于电脑性能不足导致开启垂直同步时游戏卡顿严重，那么就需要关闭垂直同步。

使用 `SetVSyncEnabled` 开启或关闭垂直同步功能。

```cpp
// 关闭垂直同步功能
Renderer::GetInstance().SetVSyncEnabled(false);
```

> 关闭垂直同步后，游戏将占满CPU，将帧数最大化，请参考 [控制主循环](./runner.md#控制主循环) 实现自定义帧数

#### 渲染资源的分配

所有与渲染相关的资源都由 Renderer 分配，例如 `画刷Brush`、`纹理Texture` 等，用户一般不需要手动调用这些方法。
