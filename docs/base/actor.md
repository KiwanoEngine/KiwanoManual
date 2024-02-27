### 角色

`角色Actor`是游戏画面里的一个个元素，例如`Sprite精灵`、`TextActor文字角色`都是一种角色，角色控制了一个游戏对象如何渲染、更新、执行动画等。

#### 角色的方法和属性

- 修改角色位置

```cpp
actor->SetPosition(Point(50, 50));
```

- 修改角色旋转角度

```cpp
actor->SetRotation(180);
```

- 将角色放大到 2 倍

```cpp
actor->SetScale(2);
```

- 设置角色透明度

```cpp
actor->SetOpacity(0.5);
```

- 获取角色大小

```cpp
float width = actor->GetWidth();
float height = actor->GetHeight();
Size size = actor->GetSize();
```

- 隐藏角色

```cpp
actor->SetVisible(false);
```

角色使用 `锚点Anchor` 来对齐，锚点是指角色的原点位置，修改锚点可以让角色变为中点对齐或任意点对齐

```cpp
// 获取舞台宽度和高度
float width = stage->GetWidth();
float height = stage->GetHeight();
// 设置角色位置为舞台宽度和高度的一半，此时角色的左上角位于舞台中心
actor->SetPosition(width / 2, height / 2);
// 修改锚点，让角色改为中心对齐，此时角色的中心位置位于舞台中心
actor->SetAnchor(0.5, 0.5);
```

角色可以有自己的名称，用来快速获取父角色下的某个特定子角色

```cpp
// 设置子角色名称
child->SetName("child actor");
// 从父角色中移除名称为 'child actor' 的角色
parent->RemoveChildren("child actor");
```

#### 角色的关系

一个角色可以有多个子角色，但只能有一个父角色，通过 `AddChild`、`RemoveChild`、`RemoveFromParent` 来添加或删除角色

```cpp
RefPtr<Actor> parent = new Actor;
RefPtr<Actor> child = new Actor;
parent->AddChild(child);
```

子角色会继承父角色的一部分属性，例如位置、透明度、旋转角度等，所以子角色的位置、透明度都是相对于父角色来说的

```cpp
// 父角色的位置设为 x=10, y=10
parent->SetPosition(Point(10, 10));
// 子角色的位置设为 x=20, y=20，但实际显示在画面的位置为 x=30, y=30
child->SetPosition(Point(20, 20));
```

使用 `GetParent` 方法可以获取父角色

```cpp
RefPtr<Actor> parent = actor->GetParent();
```

角色被添加到舞台上后，可以使用 `GetStage` 获取角色所在舞台

```cpp
RefPtr<Stage> stage = actor->GetStage();
```

#### 角色动画

角色是执行动画的单位，使用 `AddAction`、`PauseAllActions`、`ResumeAllActions`、`StopAllActions` 方法来添加、暂停、继续、停止动画

```cpp
// 让角色执行一个旋转动画，在1秒内旋转到60度
actor->AddAction(Tween::RotateTo(1_sec, 60));
// 停止角色的所有动画
actor->StopAllActions();
```

#### 事件监听

角色是事件分发与监听的单位，使用 `AddListener`、`RemoveListener` 等方法来添加、删除事件监听器

```cpp
// 添加一个鼠标按下事件的监听器
actor->AddListener<MouseDownEvent>("mouse down", [](Event*) { KGE_LOG("鼠标按下"); });
// 移除所有名称为 'mouse down' 的监听器
actor->RemoveListeners("mouse down");
```

#### 定时任务

角色可以通过 `AddTask` 来完成一些固定时间间隔的任务

```cpp
// 定时器执行的回调函数
auto callback = [](Task* t, Duration dt) { KGE_LOG("任务名称：", t->GetName(), "，时间间隔为：", dt.ToString()); };
// 添加定时器，每隔 0.5 秒执行一次 callback 函数
actor->AddTask("my task", callback, 0.5_sec);
// 根据任务名称启动、停止、移除任务
actor->StartTasks("my task");
actor->StopTasks("my task");
actor->RemoveTasks("my task");
```

#### 角色的调试

如果角色的显示不正常或行为不正常，也许角色边界渲染会帮助到你，开启边界渲染功能可以让你直观地看到角色的大小和边界

```cpp
// 让导演开启角色边界渲染功能
Director::GetInstance().SetRenderBorderEnabled(true);
// 打开角色的边界渲染
actor->ShowBorder(true);
```
