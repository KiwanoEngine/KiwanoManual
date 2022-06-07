### 用户输入

`输入Input` 可以获取到鼠标和键盘的实时按键状态。

#### 单例模式

和 `应用程序Application` 一样使用单例模式，使用 `GetInstance()` 获取该对象的引用。

```cpp
// 获取输入设备实例
Input& input = Input::GetInstance();
```

#### 按键键值

鼠标的按键键值有：

```cpp
MouseButton::Left   // 鼠标左键
MouseButton::Right  // 鼠标右键
MouseButton::Middle // 鼠标中键
```

键盘的按键键值有：

```cpp
KeyCode::A        // 字母键 A-Z
KeyCode::Num0     // 数字键 0-9
KeyCode::Numpad0  // 数字小键盘 0-9
KeyCode::F1       // F键 1-12
KeyCode::Up       // 上键
KeyCode::Left     // 左键
KeyCode::Right    // 右键
KeyCode::Down     // 下键
KeyCode::Enter    // 回车键
KeyCode::Space    // 空格键
KeyCode::Esc      // 退出键
KeyCode::Ctrl     // CTRL键
KeyCode::Shift    // SHIFT键
KeyCode::Alt      // ALT键
KeyCode::Tab      // TAB键
KeyCode::Delete   // 删除键
KeyCode::Back     // 退格键
KeyCode::Super    // Cmd|Super|Windows键
```

#### 常见函数

- 判断按键是否正被按下

```cpp
// 获取输入设备
Input& input = Input::GetInstance();
// 判断键盘空格键是否正被按下
bool spaceDown = input.IsDown(KeyCode::Space);
// 判断鼠标左键是否正被按下
bool leftDown = input.IsDown(MouseButton::Left);
```

- 判断按键是否刚被点击

```cpp
// 获取输入设备
Input& input = Input::GetInstance();
// 判断键盘空格键是否刚被点击
bool spacePressed = input.WasPressed(KeyCode::Space);
// 判断鼠标左键是否刚被点击
bool leftPressed = input.WasPressed(MouseButton::Left);
```

- 判断按键是否刚抬起

```cpp
// 获取输入设备
Input& input = Input::GetInstance();
// 判断键盘空格键是否刚抬起
bool spaceReleased = input.WasReleased(KeyCode::Space);
// 判断鼠标左键是否刚抬起
bool leftReleased = input.WasReleased(MouseButton::Left);
```

- 获取鼠标位置

```cpp
// 获取输入设备
Input& input = Input::GetInstance();
// 获取鼠标位置
Point pos = input.GetMousePos();
// 获取鼠标X位置
float x = input.GetMouseX();
// 获取鼠标Y位置
float y = input.GetMouseY();
```
