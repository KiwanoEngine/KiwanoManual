### 舞台

`舞台Stage` 是各种图形、精灵的载体，所有可见物体必须添加到舞台或其子角色中，才会被渲染出来。舞台的概念类似于游戏中的界面，例如一个游戏可能有开始界面、游戏界面、结束界面，每个界面都可以用一个舞台来表示。

#### 舞台的创建

角色只有添加到舞台上以后才会显示在画面上，所以舞台实际上是所有角色的父角色，它也具有普通角色的属性与方法。

```cpp
// 创建舞台
RefPtr<Stage> myStage = new Stage;
// 添加一个角色到舞台上
myStage->AddChild(actor1);
// 将一个角色从舞台上移除
myStage->RemoveChild(actor1);
// 将所有名称为 'actor2' 的角色移除
myStage->RemoveChildren("actor2");
```

舞台可以和普通角色一样监听事件、执行定时任务等。

#### 舞台的进入与退出

舞台的进入与退出是由 `导演Director` 决定的，使用导演的 `EnterStage` 方法切换舞台

```cpp
// 切换到 myStage 舞台
Director::GetInstance().EnterStage(myStage);
```

重载舞台的 `OnEnter` 和 `OnExit` 函数，可以在舞台进入和退出时执行特定的函数

```cpp
class MyStage : public kiwano::Stage
{
public:
    void OnEnter() { KGE_LOG("进入了MyStage!"); }
    void OnExit() { KGE_LOG("退出了MyStage!"); }
};
```

导演可以储存一个舞台栈，具体请查阅 [舞台的切换](./director.html#舞台的切换)

#### 舞台的过度动画

导演可以在切换舞台时使用一些过渡动画，具体请查阅 [舞台的切换过渡动画](./director.html#舞台的切换过渡动画)
