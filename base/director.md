### 导演

`导演Director` 是舞台的管理者，可以控制舞台的进入退出以及过渡效果，同时导演也是事件的分发器。

#### 舞台的切换

导演可以控制舞台之间的切换，使用导演的 `EnterStage` 方法切换舞台

```cpp
// 切换到 myStage 舞台
Director::GetInstance().EnterStage(myStage);
```

导演可以保存一个舞台栈，例如从舞台1切换到舞台2之后，还可以退出舞台2返回舞台1

```cpp
// 进入舞台1
Director::GetInstance().PushStage(stage1);
// 进入舞台2
Director::GetInstance().PushStage(stage2);
// 退出当前舞台, 返回舞台1
Director::GetInstance().PopStage();
```

#### 获取当前的舞台

在代码的任何地方你都可以通过导演获取到当前的舞台，使用 `GetCurrentStage` 方法获取

```cpp
StagePtr stage = Director::GetInstance().GetCurrentStage();
```

#### 舞台的切换过渡动画

切换舞台时，可以自定义过渡动画让切换效果变得流畅

```cpp
// 创建一个时长为 1 秒的淡入淡出过渡动画
TransitionPtr transition = new FadeTransition(1_sec);
// 切换场景时使用该动画
Director::GetInstance()->EnterStage(stage, transition);
Director::GetInstance()->PushStage(stage, transition);
Director::GetInstance()->PopStage(transition);
```
