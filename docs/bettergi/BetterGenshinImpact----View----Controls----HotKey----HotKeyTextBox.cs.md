# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\HotKey\HotKeyTextBox.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.Model; 
using System.Windows; 
using System.Windows.Input; 
using KeyEventArgs = System.Windows.Input.KeyEventArgs; 
using TextBox = Wpf.Ui.Controls.TextBox;

namespace BetterGenshinImpact.View.Controls.HotKey;

# 定义继承自 TextBox 的 HotKeyTextBox 类
public class HotKeyTextBox : TextBox
{
    # 注册一个依赖属性 HotkeyTypeNameProperty
    public static readonly DependencyProperty HotkeyTypeNameProperty = DependencyProperty.Register(
        nameof(HotKeyTypeName),  # 属性名
        typeof(string),          # 属性类型
        typeof(HotKeyTextBox),   # 所属类
        new FrameworkPropertyMetadata(
            default(string),      # 默认值
            FrameworkPropertyMetadataOptions.BindsTwoWayByDefault  # 元数据选项
        )
    );

    /// <summary>
    /// 热键类型 (中文)
    /// </summary>
    public string HotKeyTypeName
    {
        get => (string)GetValue(HotkeyTypeNameProperty);  # 获取 HotkeyTypeName 的值
        set => SetValue(HotkeyTypeNameProperty, value);   # 设置 HotkeyTypeName 的值
    }

    # 注册一个依赖属性 HotkeyProperty
    public static readonly DependencyProperty HotkeyProperty = DependencyProperty.Register(
        nameof(Hotkey),            # 属性名
        typeof(Model.HotKey),      # 属性类型
        typeof(HotKeyTextBox),     # 所属类
        new FrameworkPropertyMetadata(
            default(Model.HotKey), # 默认值
            FrameworkPropertyMetadataOptions.BindsTwoWayByDefault,  # 元数据选项
            (sender, _) =>         # 属性改变时的回调
            {
                var control = (HotKeyTextBox)sender;  # 获取调用者实例
                control.Text = control.Hotkey.ToString();  # 更新 Text 属性
            }
        )
    );

    public Model.HotKey Hotkey
    {
        get => (Model.HotKey)GetValue(HotkeyProperty);  # 获取 Hotkey 的值
        set => SetValue(HotkeyProperty, value);         # 设置 Hotkey 的值
    }

    # 构造函数初始化 HotKeyTextBox 实例
    public HotKeyTextBox()
    {
        IsReadOnly = true;                # 设置文本框为只读
        IsReadOnlyCaretVisible = false;  # 隐藏只读文本框的光标
        IsUndoEnabled = false;           # 禁用撤销操作

        if (ContextMenu is not null)      # 如果 ContextMenu 不为空
            ContextMenu.Visibility = Visibility.Collapsed;  # 隐藏上下文菜单

        Text = Hotkey.ToString();        # 设置文本框的显示内容为 Hotkey 的字符串表示
    }

    # 检查键是否是字符键
    private static bool HasKeyChar(Key key) =>
        key
            is
            // A - Z
            >= Key.A
            and <= Key.Z
            or
            // 0 - 9
            >= Key.D0
            and <= Key.D9
            or
            // Numpad 0 - 9
            >= Key.NumPad0
            and <= Key.NumPad9
            or
            // 其他键
            Key.OemQuestion
            or Key.OemQuotes
            or Key.OemPlus
            or Key.OemOpenBrackets
            or Key.OemCloseBrackets
            or Key.OemMinus
            or Key.DeadCharProcessed
            or Key.Oem1
            or Key.Oem5
            or Key.Oem7
            or Key.OemPeriod
            or Key.OemComma
            or Key.Add
            or Key.Divide
            or Key.Multiply
            or Key.Subtract
            or Key.Oem102
            or Key.Decimal;

    # 处理键盘按下事件
    protected override void OnPreviewKeyDown(KeyEventArgs args)
    {
        // 标记当前事件已经被处理
        args.Handled = true;

        // 获取键盘修饰符和按键信息
        var modifiers = Keyboard.Modifiers;
        var key = args.Key;

        // 如果没有按下任何键，直接返回
        if (key == Key.None)
            return;

        // 如果按下的是系统键，需要从 SystemKey 中提取实际按键
        if (key == Key.System)
            key = args.SystemKey;

        // 如果按下的是 Delete、Backspace 或 Escape 键且没有修饰符，则清除当前值并返回
        if (key is Key.Delete or Key.Back or Key.Escape && modifiers == ModifierKeys.None)
        {
            Hotkey = Model.HotKey.None;
            return;
        }

        // 如果按下的唯一键是修饰键之一，则返回
        if (
            key
            is Key.LeftCtrl
            or Key.RightCtrl
            or Key.LeftAlt
            or Key.RightAlt
            or Key.LeftShift
            or Key.RightShift
            or Key.LWin
            or Key.RWin
            or Key.Clear
            or Key.OemClear
            or Key.Apps
        )
            return;

        // 如果按下的是 Enter、Space 或 Tab 键且没有修饰符，则返回
        if (key is Key.Enter or Key.Tab && modifiers == ModifierKeys.None)
            return;

        // 如果 HotKeyTypeName 为 GlobalRegister 并且按下的是 Enter、Space 或 Tab 键且没有修饰符，则返回
        if (HotKeyTypeName == HotKeyTypeEnum.GlobalRegister.ToChineseName() && key is Key.Enter or Key.Space or Key.Tab && modifiers == ModifierKeys.None)
            return;

        // 如果 HotKeyTypeName 为 GlobalRegister 并且按下的键有字符，且没有修饰符或仅有 Shift 修饰符，则返回
        if (HotKeyTypeName == HotKeyTypeEnum.GlobalRegister.ToChineseName() && HasKeyChar(key) && modifiers is ModifierKeys.None or ModifierKeys.Shift)
            return;

        // 设置热键值
        Hotkey = new Model.HotKey(key, modifiers);
    }

    /// <summary>
    /// 支持鼠标侧键配置
    /// </summary>
    /// <param name="args"></param>
    protected override void OnPreviewMouseDown(MouseButtonEventArgs args)
    {
        // 如果按下的是鼠标的第一个或第二个侧键
        if (args.ChangedButton is MouseButton.XButton1 or MouseButton.XButton2)
        {
            // 如果 HotKeyTypeName 为 GlobalRegister，则设置热键为无
            if (HotKeyTypeName == HotKeyTypeEnum.GlobalRegister.ToChineseName())
            {
                Hotkey = new Model.HotKey(Key.None);
            }
            else
            {
                // 否则，设置热键为无键并且没有修饰符，同时记录侧键
                Hotkey = new Model.HotKey(Key.None, ModifierKeys.None, args.ChangedButton);
            }
        }
    }
# 这是代码块的结束括号，用于闭合代码块
}
```