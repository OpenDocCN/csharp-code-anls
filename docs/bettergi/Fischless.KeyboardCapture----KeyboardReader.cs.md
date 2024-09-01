# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardReader.cs`

```cs
# 引入系统诊断工具
﻿using System.Diagnostics;

# 定义命名空间
namespace Fischless.KeyboardCapture;

# 使用调试显示属性，显示结果对象的字符串表示
[DebuggerDisplay("{result.ToString()}")]
# 定义键盘读取器类，实现 IDisposable 接口以支持资源释放
public class KeyboardReader : IDisposable
{
    # 定义静态默认实例
    public static KeyboardReader Default { get; } = new();

    # 定义事件，当键盘结果被接收到时触发
    public event EventHandler<KeyboardResult> Received = null!;

    # 定义是否仅组合键的属性
    public bool IsCombinationOnly { get; set; } = false;
    # 定义是否区分大小写的属性
    public bool IsCaseSensitived { get; set; } = false;
    # 定义是否按下 Shift 键的属性（只读）
    public bool IsShift { get; private set; } = false;
    # 定义是否按下 Ctrl 键的属性（只读）
    public bool IsCtrl { get; private set; } = false;
    # 定义是否按下 Alt 键的属性（只读）
    public bool IsAlt { get; private set; } = false;
    # 定义是否按下 Win 键的属性（只读）
    public bool IsWin { get; private set; } = false;
    # 定义是否按下扩展键的属性（只读）
    public bool IsExtendedKey { get; private set; } = false;

    # 定义键盘挂钩对象
    protected KeyboardHook KeyboardHook = new();

    # 构造函数，启动键盘读取器
    public KeyboardReader()
    {
        Start();
    }

    # 析构函数，调用 Dispose 方法以释放资源
    ~KeyboardReader()
    {
        Dispose();
    }

    # 实现 IDisposable 接口的 Dispose 方法
    public void Dispose()
    {
        Stop();
    }

    # 启动键盘挂钩，注册和启动事件处理
    public void Start()
    {
        KeyboardHook.KeyDown -= OnKeyDown;
        KeyboardHook.KeyDown += OnKeyDown;
        KeyboardHook.KeyUp -= OnKeyUp;
        KeyboardHook.KeyUp += OnKeyUp;
        KeyboardHook.Start();
    }

    # 停止键盘挂钩，取消事件处理
    public void Stop()
    {
        KeyboardHook.Stop();
        KeyboardHook.KeyDown -= OnKeyDown;
        KeyboardHook.KeyUp -= OnKeyUp;
    }

    # 按下键盘时调用的方法（方法体未完成）
    private void OnKeyDown(object? sender, KeyboardEventArgs e)
    {
        # 检查是否按下 Shift 键的任意变体
        if (e.KeyCode == KeyboardKeys.Shift
         || e.KeyCode == KeyboardKeys.ShiftKey
         || e.KeyCode == KeyboardKeys.LShiftKey
         || e.KeyCode == KeyboardKeys.RShiftKey)
        {
            # 如果 Shift 键尚未被标记为按下
            if (!IsShift)
            {
                # 将 IsShift 标记为真
                IsShift = true;
                # 标记是否按下的是右侧 Shift 键
                IsExtendedKey = e.KeyCode == KeyboardKeys.RShiftKey;
                # 如果仅为组合键，则返回，不继续执行后续代码
                if (IsCombinationOnly)
                {
                    return;
                }
            }
        }
        # 检查是否按下 Ctrl 键的任意变体
        else if (e.KeyCode == KeyboardKeys.Control
              || e.KeyCode == KeyboardKeys.ControlKey
              || e.KeyCode == KeyboardKeys.LControlKey
              || e.KeyCode == KeyboardKeys.RControlKey)
        {
            # 如果 Ctrl 键尚未被标记为按下
            if (!IsCtrl)
            {
                # 将 IsCtrl 标记为真
                IsCtrl = true;
                # 标记是否按下的是右侧 Ctrl 键
                IsExtendedKey = e.KeyCode == KeyboardKeys.RControlKey;
                # 如果仅为组合键，则返回，不继续执行后续代码
                if (IsCombinationOnly)
                {
                    return;
                }
            }
        }
        # 检查是否按下 Win 键的任意变体
        else if (e.KeyCode == KeyboardKeys.LWin
              || e.KeyCode == KeyboardKeys.RWin)
        {
            # 如果 Win 键尚未被标记为按下
            if (!IsWin)
            {
                # 将 IsWin 标记为真
                IsWin = true;
                # 标记是否按下的是右侧 Win 键
                IsExtendedKey = e.KeyCode == KeyboardKeys.RWin;
                # 如果仅为组合键，则返回，不继续执行后续代码
                if (IsCombinationOnly)
                {
                    return;
                }
            }
        }
        # 检查是否按下 Alt 键的任意变体
        else if (e.KeyCode == KeyboardKeys.Alt
              || e.KeyCode == KeyboardKeys.LMenu
              || e.KeyCode == KeyboardKeys.RMenu)
        {
            # 如果 Alt 键尚未被标记为按下
            if (!IsAlt)
            {
                # 将 IsAlt 标记为真
                IsAlt = true;
                # 标记是否按下的是右侧 Alt 键
                IsExtendedKey = e.KeyCode == KeyboardKeys.RMenu;
                # 如果仅为组合键，则返回，不继续执行后续代码
                if (IsCombinationOnly)
                {
                    return;
                }
            }
        }
        # 检查是否按下回车键
        else if (e.KeyCode == KeyboardKeys.Return)
        {
            # 标记是否按下的是扩展的回车键
            IsExtendedKey = KeyboardKeys.Return.IsExtendedKey();
        }

        # 获取当前的日期和时间
        DateTime now = DateTime.Now;
#if false
        # 如果条件为 false，则不执行以下调试输出
        Debug.WriteLine(e.KeyCode);
#endif

        # 声明一个 KeyboardItem 类型的变量 item
        KeyboardItem item;

        # 根据是否区分大小写来处理键盘事件
        if (IsCaseSensitived)
        {
            # 判断当前是否是大写状态
            bool isUpper = KeyboardKeys.CapsLock.IsKeyLocked() ? !IsShift : IsShift;

            # 根据是否大写，创建对应的 KeyboardItem 实例
            if (isUpper)
            {
                item = new KeyboardItem(now, e.KeyCode, char.ToUpper((char)e.KeyCode).ToString());
            }
            else
            {
                item = new KeyboardItem(now, e.KeyCode, char.ToLower((char)e.KeyCode).ToString());
            }
        }
        else
        {
            # 如果不区分大小写，则创建不包含字符表示的 KeyboardItem 实例
            item = new KeyboardItem(now, e.KeyCode);
        }

        # 创建 KeyboardResult 对象并设置其属性
        KeyboardResult result = new(item)
        {
            IsShift = IsShift,
            IsCtrl = IsCtrl,
            IsAlt = IsAlt,
            IsWin = IsWin,
            IsExtendedKey = IsExtendedKey,
        };

        # 调用 Received 事件（如果有订阅者的话），传递当前对象和结果
        Received?.Invoke(this, result);
#if true
        # 如果条件为 true，则执行调试输出
        Debug.WriteLine(result.ToString());
#endif
    }

    # 键盘按键释放事件处理方法
    private void OnKeyUp(object? sender, KeyboardEventArgs e)
    {
        # 处理 Shift 键释放事件
        if (e.KeyCode == KeyboardKeys.Shift
         || e.KeyCode == KeyboardKeys.ShiftKey
         || e.KeyCode == KeyboardKeys.LShiftKey
         || e.KeyCode == KeyboardKeys.RShiftKey)
        {
            IsShift = false; # 将 IsShift 标记设置为 false
            IsExtendedKey = e.KeyCode == KeyboardKeys.RShiftKey; # 设置是否是扩展键
            return;
        }
        # 处理 Control 键释放事件
        else if (e.KeyCode == KeyboardKeys.Control
              || e.KeyCode == KeyboardKeys.ControlKey
              || e.KeyCode == KeyboardKeys.LControlKey
              || e.KeyCode == KeyboardKeys.RControlKey)
        {
            IsExtendedKey = e.KeyCode == KeyboardKeys.RControlKey; # 设置是否是扩展键
            IsCtrl = false; # 将 IsCtrl 标记设置为 false
            return;
        }
        # 处理 Windows 键释放事件
        else if (e.KeyCode == KeyboardKeys.LWin
              || e.KeyCode == KeyboardKeys.RWin)
        {
            IsExtendedKey = e.KeyCode == KeyboardKeys.RWin; # 设置是否是扩展键
            IsWin = false; # 将 IsWin 标记设置为 false
            return;
        }
        # 处理 Alt 键释放事件
        else if (e.KeyCode == KeyboardKeys.Alt
              || e.KeyCode == KeyboardKeys.LMenu
              || e.KeyCode == KeyboardKeys.RMenu)
        {
            IsExtendedKey = e.KeyCode == KeyboardKeys.RMenu; # 设置是否是扩展键
            IsAlt = false; # 将 IsAlt 标记设置为 false
            return;
        }
        # 处理 Enter 键释放事件
        else if (e.KeyCode == KeyboardKeys.Return)
        {
            IsExtendedKey = KeyboardKeys.Return.IsExtendedKey(); # 更新是否是扩展键的标记
        }
    }
}
```