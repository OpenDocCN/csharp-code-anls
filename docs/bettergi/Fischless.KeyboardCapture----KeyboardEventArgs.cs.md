# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardEventArgs.cs`

```cs
# 定义一个委托，用于处理键盘事件
public delegate void KeyboardEventHandler(object? sender, KeyboardEventArgs e);

# 定义一个委托，用于处理键盘按下事件
public delegate void KeyboardPressEventHandler(object? sender, KeyboardPressEventArgs e);

# 定义一个表示键盘事件参数的类
public class KeyboardEventArgs
{
    # 私有字段，用于存储是否抑制按键事件
    private bool _suppressKeyPress;

    # 构造函数，初始化 KeyboardEventArgs 类的实例
    public KeyboardEventArgs(KeyboardKeys keyData)
    {
        KeyData = keyData;
    }

    # 判断是否按下 Alt 键
    public virtual bool Alt => (KeyData & KeyboardKeys.Alt) == KeyboardKeys.Alt;

    # 判断是否按下 Control 键
    public bool Control => (KeyData & KeyboardKeys.Control) == KeyboardKeys.Control;

    # 表示是否已处理此事件
    public bool Handled { get; set; }

    # 获取按键代码
    public KeyboardKeys KeyCode
    {
        get
        {
            # 从 KeyData 中提取键代码
            KeyboardKeys keyGenerated = KeyData & KeyboardKeys.KeyCode;

            # 检查键代码是否在枚举定义中
            if (!Enum.IsDefined(typeof(KeyboardKeys), (int)keyGenerated))
            {
                return KeyboardKeys.None;
            }

            return keyGenerated;
        }
    }

    # 获取按键值
    public int KeyValue => (int)(KeyData & KeyboardKeys.KeyCode);

    # 获取按键数据
    public KeyboardKeys KeyData { get; }

    # 获取修饰键（如 Shift、Ctrl、Alt）
    public KeyboardKeys Modifiers => KeyData & KeyboardKeys.Modifiers;

    # 判断是否按下 Shift 键
    public virtual bool Shift => (KeyData & KeyboardKeys.Shift) == KeyboardKeys.Shift;

    # 是否抑制按键事件的标志
    public bool SuppressKeyPress
    {
        get => _suppressKeyPress;
        set
        {
            _suppressKeyPress = value;
            # 设置 Handled 标志，表明事件是否被处理
            Handled = value;
        }
    }
}

# 定义一个表示键盘按下事件参数的类
public class KeyboardPressEventArgs : EventArgs
{
    # 构造函数，初始化 KeyboardPressEventArgs 类的实例
    public KeyboardPressEventArgs(char keyChar)
    {
        KeyChar = keyChar;
    }

    # 按键字符
    public char KeyChar { get; set; }
    
    # 表示是否已处理此事件
    public bool Handled { get; set; }
}
```