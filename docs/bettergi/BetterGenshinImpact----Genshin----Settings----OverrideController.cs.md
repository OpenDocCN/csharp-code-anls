# `.\better-genshin-impact\BetterGenshinImpact\Genshin\Settings\OverrideController.cs`

```cs
# 引用外部命名空间和类
﻿using Fischless.WindowsInput;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Runtime.InteropServices;
using System.Windows.Forms;
using System.Xml.Linq;
using static Vanara.PInvoke.User32;

# 定义 BetterGenshinImpact.Genshin.Settings 命名空间
namespace BetterGenshinImpact.Genshin.Settings;

# 定义 OverrideControllerSettings 类
public sealed class OverrideControllerSettings
{
    # 定义可空的 KeyboardMap 属性
    public KeyboardMap? KeyboardMap;

    # 构造函数，接受 MainJson 类型的参数
    public OverrideControllerSettings(MainJson data)
    {
        # 检查 OverrideControllerMapKeyList 和 OverrideControllerMapValueList 是否为 null，以及它们的长度是否相等
        if (data.OverrideControllerMapKeyList == null
         || data.OverrideControllerMapValueList == null
         || data.OverrideControllerMapKeyList.Length != data.OverrideControllerMapValueList.Length)
        {
            return;
        }

        # 遍历 OverrideControllerMapKeyList
        for (int i = default; i < data.OverrideControllerMapKeyList.Length; i++)
        {
            string? key = data.OverrideControllerMapKeyList[i];

            # 如果找到特定的键
            if (key == "OverrideControllerMap__00000000-0000-0000-0000-000000000000__0")
            {
                # 解析对应的 XML 数据
                string xmlRaw = data.OverrideControllerMapValueList[i];
                XDocument xmlDoc = XDocument.Parse(xmlRaw);

                # 如果 XML 根元素名称为 "KeyboardMap"，则创建 KeyboardMap 实例
                if (xmlDoc?.Root?.Name.LocalName == "KeyboardMap")
                {
                    KeyboardMap = new(xmlRaw);
                }

                # 只检测 KeyboardMap，跳出循环
                break;
            }
        }
    }
}

# 定义 KeyboardMap 类
public sealed class KeyboardMap
{
    # 定义只读字段
    private readonly int? sourceMapId = null;
    private readonly int? categoryId = null;
    private readonly int? layoutId = null;
    private readonly string? name = null;
    private readonly string? hardwareGuid = null;
    private readonly bool? enabled = null;

    # 定义只读属性
    public int? SourceMapId => sourceMapId;
    public int? CategoryId => categoryId;
    public int? LayoutId => layoutId;
    public string? Name => name;
    public string? HardwareGuid => hardwareGuid;
    public bool? Enabled => enabled;
    # 定义 ActionElementMap 类型的列表属性
    public List<ActionElementMap> ActionElementMap { get; private set; } = [];

    # 构造函数，接受可空的 XML 数据
    public KeyboardMap(string? xmlRaw)
    {
        # 如果 XML 数据为空或只包含空白字符，则返回
        if (string.IsNullOrWhiteSpace(xmlRaw))
        {
            return;
        }

        try
        {
            # 解析 XML 数据
            XElement xml = XElement.Parse(xmlRaw);
            sourceMapId = (int?)xml?.ParseItem<int>(nameof(sourceMapId));
            categoryId = (int?)xml?.ParseItem<int>(nameof(categoryId));
            layoutId = (int?)xml?.ParseItem<int>(nameof(layoutId));
            name = (string?)xml?.ParseItem<string>(nameof(name));
            hardwareGuid = (string?)xml?.ParseItem<string>(nameof(hardwareGuid));
            enabled = (bool?)xml?.ParseItem<bool>(nameof(enabled));

            # 遍历 XML 中的 ActionElementMap 元素，并添加到列表中
            foreach (XElement mapItem in xml?.ParseList(nameof(ActionElementMap)) ?? [])
            {
                ActionElementMap item = new(mapItem.ToString());
                ActionElementMap.Add(item);
            }
        }
        catch (Exception e)
        {
            # 捕获异常并输出调试信息
            Debug.WriteLine(e);
        }
    }
}

# 定义 ActionElementMap 类
public sealed class ActionElementMap
{
    // 定义只读的可空整型字段，用于存储不同的动作分类 ID
    private readonly int? actionCategoryId = null;
    // 定义只读的可空整型字段，用于存储动作 ID
    private readonly int? actionId = null;
    // 定义只读的可空整型字段，用于存储元素类型
    private readonly int? elementType = null;
    // 定义只读的可空整型字段，用于存储元素标识符 ID
    private readonly int? elementIdentifierId = null;
    // 定义只读的可空整型字段，用于存储轴范围
    private readonly int? axisRange = null;
    // 定义只读的可空整型字段，用于存储是否反转
    private readonly int? invert = null;
    // 定义只读的可空整型字段，用于存储轴贡献
    private readonly int? axisContribution = null;
    // 定义只读的可空整型字段，用于存储键盘键码
    private readonly int? keyboardKeyCode = null;
    // 定义只读的可空整型字段，用于存储修饰键 1
    private readonly int? modifierKey1 = null;
    // 定义只读的可空整型字段，用于存储修饰键 2
    private readonly int? modifierKey2 = null;
    // 定义只读的可空整型字段，用于存储修饰键 3
    private readonly int? modifierKey3 = null;
    // 定义只读的可空布尔型字段，用于存储是否启用
    private readonly bool? enabled = null;

    // 属性：获取动作分类 ID
    public int? ActionCategoryId => actionCategoryId;
    // 属性：获取动作 ID，并将其转换为 ActionId 类型
    public ActionId? ActionId => (ActionId?)actionId;
    // 属性：获取元素类型
    public int? ElementType => elementType;
    // 属性：获取元素标识符 ID，并将其转换为 ElementIdentifierId 类型
    public ElementIdentifierId? ElementIdentifierId => (ElementIdentifierId?)elementIdentifierId;
    // 属性：获取轴范围
    public int? AxisRange => axisRange;
    // 属性：获取是否反转
    public int? Invert => invert;
    // 属性：获取轴贡献
    public int? AxisContribution => axisContribution;
    // 属性：获取键盘键码，并将其转换为 Keys 类型
    public Keys? KeyboardKeyCode => (Keys?)keyboardKeyCode;
    // 属性：获取是否启用
    public bool? Enabled => enabled;

    // 属性：表示是否按下了 Ctrl 键
    public bool IsCtrl { get; set; }
    // 属性：表示是否按下了 Shift 键
    public bool IsShift { get; set; }
    // 属性：表示是否按下了 Alt 键
    public bool IsAlt { get; set; }

    // 构造函数：接受一个 XML 字符串并初始化字段
    public ActionElementMap(string? xmlRaw)
    {
        // 如果 XML 字符串为空或仅包含空白字符，则直接返回
        if (string.IsNullOrWhiteSpace(xmlRaw))
        {
            return;
        }

        try
        {
            // 解析 XML 字符串
            XElement xml = XElement.Parse(xmlRaw);
            // 从 XML 中解析并设置字段的值
            actionCategoryId = (int?)xml?.ParseItem<int>(nameof(actionCategoryId));
            actionId = (int?)xml?.ParseItem<int>(nameof(actionId));
            elementType = (int?)xml?.ParseItem<int>(nameof(elementType));
            elementIdentifierId = (int?)xml?.ParseItem<int>(nameof(elementIdentifierId));
            axisRange = (int?)xml?.ParseItem<int>(nameof(axisRange));
            invert = (int?)xml?.ParseItem<int>(nameof(invert));
            axisContribution = (int?)xml?.ParseItem<int>(nameof(axisContribution));
            keyboardKeyCode = (int?)xml?.ParseItem<int>(nameof(keyboardKeyCode));
            modifierKey1 = (int?)xml?.ParseItem<int>(nameof(modifierKey1));
            modifierKey2 = (int?)xml?.ParseItem<int>(nameof(modifierKey2));
            modifierKey3 = (int?)xml?.ParseItem<int>(nameof(modifierKey3));
            enabled = (bool?)xml?.ParseItem<bool>(nameof(enabled));

            // 遍历修饰键的值并设置相应的键状态
            foreach (int? mod in new List<int?> { modifierKey1, modifierKey2, modifierKey3 })
            {
                switch (mod)
                {
                    case 1:
                        IsCtrl = true;
                        break;

                    case 2:
                        IsAlt = true;
                        break;

                    case 3:
                        IsShift = true;
                        break;

                    case null:
                    default:
                        break;
                }
            }
        }
        // 捕获异常并输出到调试窗口
        catch (Exception e)
        {
            Debug.WriteLine(e);
        }
    }
    // 重写 ToString 方法以返回对象的字符串表示
    public override string ToString()
        // 使用字符串插值创建格式化的字符串，其中包含 ActionId、KeyboardKeyCode 和 ElementIdentifierId
        => $"{ActionId} : {KeyboardKeyCode} : {ElementIdentifierId}" +
           // 根据 IsCtrl 的值决定是否在字符串中包含 "Ctrl"
           $" - {(IsCtrl ? "Ctrl" : string.Empty)}" +
           // 根据 IsShift 的值决定是否在字符串中包含 "Shift"
           $" {(IsShift ? "Shift" : string.Empty)}" +
           // 根据 IsAlt 的值决定是否在字符串中包含 "Alt"
           $" {(IsAlt ? "Alt" : string.Empty)}";
}  // 结束前一个代码块的右大括号

file static class XmlParseExtension  // 定义一个静态类 XmlParseExtension，用于 XML 解析扩展
{
    // 定义一个泛型静态方法 ParseItem，接收 XML 元素和键名，解析对应的值
    public static dynamic? ParseItem<T>([In] this XElement xml, [In] string keyName)
    {
        // 获取 XML 元素中指定名称的第一个子元素
        XElement el = xml.Descendants()
                         .Where(el => el.Name.LocalName == keyName)
                         .First();

        // 判断泛型 T 是否为 int 类型
        if (typeof(T) == typeof(int))
        {
            // 尝试将子元素的值解析为 int 类型，并返回解析结果
            if (int.TryParse(el.Value, out int result))
            {
                return result;
            }
        }
        // 判断泛型 T 是否为 bool 类型
        else if (typeof(T) == typeof(bool))
        {
            // 尝试将子元素的值解析为 bool 类型，并返回解析结果
            if (bool.TryParse(el.Value, out bool result))
            {
                return result;
            }
        }
        // 判断泛型 T 是否为 string 类型
        else if (typeof(T) == typeof(string))
        {
            // 直接返回子元素的值
            return el.Value;
        }
    # 挑战模式放弃游戏手柄按钮
    AbandonChallengeGamepad = 85,
    # 照片隐藏 UI 按钮
    PhotoHideUI = 86,
    # 小工具按钮
    Gadget = 87,
    # 一些交互模式按钮
    InteractionSomeModes = 88,
    # 额外上按钮
    ExtraUp = 89,
    # 额外下按钮
    ExtraDown = 90,
    # 额外左按钮
    ExtraLeft = 91,
    # 额外右按钮
    ExtraRight = 92,
    # 音乐左上按钮
    MusicLeftUp = 94,
    # 音乐左右按钮
    MusicLeftRight = 95,
    # 音乐左下按钮
    MusicLeftDown = 96,
    # 音乐左左按钮
    MusicLeftLeft = 97,
    # 音乐右上按钮
    MusicRightUp = 98,
    # 音乐右右按钮
    MusicRightRight = 99,
    # 音乐右下按钮
    MusicRightDown = 100,
    # 音乐右左按钮
    MusicRightLeft = 101,
    # 音乐音符 11
    MusicNote11 = 102,
    # 音乐音符 12
    MusicNote12 = 103,
    # 音乐音符 13
    MusicNote13 = 104,
    # 音乐音符 14
    MusicNote14 = 105,
    # 音乐音符 15
    MusicNote15 = 106,
    # 音乐音符 16
    MusicNote16 = 107,
    # 音乐音符 17
    MusicNote17 = 108,
    # 音乐音符 21
    MusicNote21 = 109,
    # 音乐音符 22
    MusicNote22 = 110,
    # 音乐音符 23
    MusicNote23 = 111,
    # 音乐音符 24
    MusicNote24 = 112,
    # 音乐音符 25
    MusicNote25 = 113,
    # 音乐音符 26
    MusicNote26 = 114,
    # 音乐音符 27
    MusicNote27 = 115,
    # 音乐音符 31
    MusicNote31 = 116,
    # 音乐音符 32
    MusicNote32 = 117,
    # 音乐音符 33
    MusicNote33 = 118,
    # 音乐音符 34
    MusicNote34 = 119,
    # 音乐音符 35
    MusicNote35 = 120,
    # 音乐音符 36
    MusicNote36 = 121,
    # 音乐音符 37
    MusicNote37 = 122,
    # F1 功能键
    F1 = 124,
    # F2 功能键
    F2 = 125,
    # F3 功能键
    F3 = 126,
    # 返回按钮
    Return = 127,
    # 左摇杆移动
    LeftStickMove = 128,
    # Pot 任务按钮
    PotTasks = 129,
    # Pot 编辑按钮
    PotEdit = 130,
    # 聚会设置按钮
    PartySetup = 131,
    # 好友按钮
    Friends = 132,
    # Pot 对象旋转上按钮
    PotObjectTurnUp = 133,
    # Pot 对象旋转下按钮
    PotObjectTurnDown = 134,
    # Pot 对象旋转左按钮
    PotObjectTurnLeft = 135,
    # Pot 对象旋转右按钮
    PotObjectTurnRight = 136,
    # 右摇杆移动 2
    RightStickMove2 = 137,
    # 左鼠标按钮
    LMB = 138,
    # 右鼠标按钮
    RMB = 139,
    # 创建自定义套件按钮
    CreateCustomSuite = 141,
}
# 结束前一个代码块（可能是类或接口的定义）

public enum ElementIdentifierId
{
    # 定义一个枚举类型，用于表示各种按键或操作的标识符

    None = 0, # 无按键或操作的标识符
    Backspace = 55, # Backspace 键的标识符
    Tab = 56, # Tab 键的标识符
    Clear = 57, # Clear 键的标识符
    Enter = 58, # Enter 键的标识符
    Pause = 59, # Pause 键的标识符
    ESC = 60, # ESC 键的标识符
    Space = 54, # 空格键的标识符
    Apostrophe = 66, # 单引号键的标识符
    Equal = 78, # 等号键的标识符
    Comma = 71, # 逗号键的标识符
    Minus = 72, # 减号键的标识符
    Period = 73, # 句号键的标识符
    Slash = 74, # 斜杠键的标识符
    D0 = 27, # 数字键 0 的标识符
    D1 = 28, # 数字键 1 的标识符
    D2 = 29, # 数字键 2 的标识符
    D3 = 30, # 数字键 3 的标识符
    D4 = 31, # 数字键 4 的标识符
    D5 = 32, # 数字键 5 的标识符
    D6 = 33, # 数字键 6 的标识符
    D7 = 34, # 数字键 7 的标识符
    D8 = 35, # 数字键 8 的标识符
    D9 = 36, # 数字键 9 的标识符
    Semicolon = 76, # 分号键的标识符
    LeftSquareBracket = 82, # 左方括号键的标识符
    Backslash = 83, # 反斜杠键的标识符
    RightSquareBracket = 84, # 右方括号键的标识符
    Tilde = 87, # 波浪号键的标识符
    A = 1, # 字母 A 键的标识符
    B = 2, # 字母 B 键的标识符
    C = 3, # 字母 C 键的标识符
    D = 4, # 字母 D 键的标识符
    E = 5, # 字母 E 键的标识符
    F = 6, # 字母 F 键的标识符
    G = 7, # 字母 G 键的标识符
    H = 8, # 字母 H 键的标识符
    I = 9, # 字母 I 键的标识符
    J = 10, # 字母 J 键的标识符
    K = 11, # 字母 K 键的标识符
    L = 12, # 字母 L 键的标识符
    M = 13, # 字母 M 键的标识符
    N = 14, # 字母 N 键的标识符
    O = 15, # 字母 O 键的标识符
    P = 16, # 字母 P 键的标识符
    Q = 17, # 字母 Q 键的标识符
    R = 18, # 字母 R 键的标识符
    S = 19, # 字母 S 键的标识符
    T = 20, # 字母 T 键的标识符
    U = 21, # 字母 U 键的标识符
    V = 22, # 字母 V 键的标识符
    W = 23, # 字母 W 键的标识符
    X = 24, # 字母 X 键的标识符
    Y = 25, # 字母 Y 键的标识符
    Z = 26, # 字母 Z 键的标识符
    Delete = 88, # Delete 键的标识符
    Numpad0 = 37, # 数字小键盘 0 的标识符
    Numpad1 = 38, # 数字小键盘 1 的标识符
    Numpad2 = 39, # 数字小键盘 2 的标识符
    Numpad3 = 40, # 数字小键盘 3 的标识符
    Numpad4 = 41, # 数字小键盘 4 的标识符
    Numpad5 = 42, # 数字小键盘 5 的标识符
    Numpad6 = 43, # 数字小键盘 6 的标识符
    Numpad7 = 44, # 数字小键盘 7 的标识符
    Numpad8 = 45, # 数字小键盘 8 的标识符
    Numpad9 = 46, # 数字小键盘 9 的标识符
    NumpadDot = 47, # 数字小键盘点的标识符
    NumpadSlash = 48, # 数字小键盘斜杠的标识符
    NumpadAsterisk = 49, # 数字小键盘星号的标识符
    NumpadMinus = 50, # 数字小键盘减号的标识符
    NumpadPlus = 51, # 数字小键盘加号的标识符
    NumpadEnter = 52, # 数字小键盘 Enter 键的标识符
    ArrowUp = 89, # 向上箭头键的标识符
    ArrowDown = 90, # 向下箭头键的标识符
    ArrowRight = 91, # 向右箭头键的标识符
    ArrowLeft = 92, # 向左箭头键的标识符
    Insert = 93, # Insert 键的标识符
    Home = 94, # Home 键的标识符
    End = 95, # End 键的标识符
    PageUp = 96, # Page Up 键的标识符
    PageDown = 97, # Page Down 键的标识符
    F1 = 98, # F1 键的标识符
    F2 = 99, # F2 键的标识符
    F3 = 100, # F3 键的标识符
    F4 = 101, # F4 键的标识符
    F5 = 102, # F5 键的标识符
    F6 = 103, # F6 键的标识符
    F7 = 104, # F7 键的标识符
    F8 = 105, # F8 键的标识符
    F9 = 106, # F9 键的标识符
    F10 = 107, # F10 键的标识符
    F11 = 108, # F11 键的标识符
    F12 = 109, # F12 键的标识符
    F13 = 110, # F13 键的标识符
    F14 = 111, # F14 键的标识符
    F15 = 112, # F15 键的标识符
    NumLock = 113, # Num Lock 键的标识符
    CapsLock = 114, # Caps Lock 键的标识符
    ScrollLock = 115, # Scroll Lock 键的标识符
    RightShift = 116, # 右 Shift 键的标识符
    LeftShift = 117, # 左 Shift 键的标识符
    RightCtrl = 118, # 右 Ctrl 键的标识符
    LeftCtrl = 119, # 左 Ctrl 键的标识符
    RightAlt = 120, # 右 Alt 键的标识符
    LeftAlt = 121, # 左 Alt 键的标识符
    RightWin = 125, # 右 Win 键的标识符
    LeftWin = 124, # 左 Win 键的标识符
    Help = 127, # Help 键的标识符
    Print = 128, # Print 键的标识符
}
# 结束枚举定义
```