# `.\better-genshin-impact\Fischless.WindowsInput\InputBuilder.cs`

```cs
﻿using System.Collections;
using Vanara.PInvoke;

namespace Fischless.WindowsInput;

// InputBuilder 类用于构建和管理输入事件
internal class InputBuilder : IEnumerable<User32.INPUT>, IEnumerable
{
    // 构造函数，初始化输入事件列表
    public InputBuilder()
    {
        _inputList = [];
    }

    // 将输入事件列表转换为数组
    public User32.INPUT[] ToArray()
    {
        return [.. _inputList];
    }

    // 返回输入事件列表的枚举器
    public IEnumerator<User32.INPUT> GetEnumerator()
    {
        return _inputList.GetEnumerator();
    }

    // 返回输入事件列表的枚举器 (IEnumerable 接口实现)
    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }

    // 索引器，根据位置获取输入事件
    public User32.INPUT this[int position] => _inputList[position];

    // 静态方法，检查给定的虚拟键码是否是扩展键
    public static bool IsExtendedKey(User32.VK keyCode)
    {
        return
            keyCode == User32.VK.VK_MENU
         || keyCode == User32.VK.VK_LMENU
         || keyCode == User32.VK.VK_RMENU
         || keyCode == User32.VK.VK_CONTROL
         || keyCode == User32.VK.VK_RCONTROL
         || keyCode == User32.VK.VK_INSERT
         || keyCode == User32.VK.VK_DELETE
         || keyCode == User32.VK.VK_HOME
         || keyCode == User32.VK.VK_END
         || keyCode == User32.VK.VK_PRIOR
         || keyCode == User32.VK.VK_NEXT
         || keyCode == User32.VK.VK_RIGHT
         || keyCode == User32.VK.VK_UP
         || keyCode == User32.VK.VK_LEFT
         || keyCode == User32.VK.VK_DOWN
         || keyCode == User32.VK.VK_NUMLOCK
         || keyCode == User32.VK.VK_CANCEL
         || keyCode == User32.VK.VK_SNAPSHOT
         || keyCode == User32.VK.VK_DIVIDE;
    }

    // 添加按键按下事件到输入列表
    public InputBuilder AddKeyDown(User32.VK keyCode, bool? isExtendedKey = null)
    {
        // 判断是否使用扩展键标志，如果未指定，则通过 IsExtendedKey 方法判断
        bool isUseExtendedKey = isExtendedKey == null ? IsExtendedKey(keyCode) : isExtendedKey.Value;

        // 特殊处理 VK_NUMPAD_ENTER，将其转换为 VK_RETURN
        if ((VK2)keyCode == VK2.VK_NUMPAD_ENTER)
        {
            keyCode = User32.VK.VK_RETURN;
            isUseExtendedKey = true;
        }

        // 创建一个新的 INPUT 结构体，用于描述键盘输入事件
        User32.INPUT input = new()
        {
            type = User32.INPUTTYPE.INPUT_KEYBOARD, // 设置事件类型为键盘输入
            ki = new User32.KEYBDINPUT() // 初始化键盘输入结构体
            {
                wVk = (ushort)keyCode, // 设置虚拟键码
                wScan = (ushort)(User32.MapVirtualKey((uint)keyCode, 0) & 0xFFU), // 获取扫描码
                dwFlags = (isUseExtendedKey ? User32.KEYEVENTF.KEYEVENTF_EXTENDEDKEY : 0), // 设置扩展键标志
                time = 0, // 设置事件时间为0
                dwExtraInfo = IntPtr.Zero, // 设置额外信息为0
            },
        };

        // 将输入事件添加到输入列表
        User32.INPUT item = input;
        _inputList.Add(item);
        return this; // 返回当前实例以支持链式调用
    }

    // 添加按键释放事件到输入列表
    # 根据是否提供扩展键标志来决定是否使用扩展键，若未提供则通过 keyCode 计算
    bool isUseExtendedKey = isExtendedKey == null ? IsExtendedKey(keyCode) : isExtendedKey.Value;

    # 如果 keyCode 是 VK_NUMPAD_ENTER，则将其转换为 VK_RETURN 并标记为使用扩展键
    if ((VK2)keyCode == VK2.VK_NUMPAD_ENTER)
    {
        keyCode = User32.VK.VK_RETURN;
        isUseExtendedKey = true;
    }

    # 创建一个新的 INPUT 结构体实例，设置其为键盘输入类型
    User32.INPUT input = new()
    {
        type = User32.INPUTTYPE.INPUT_KEYBOARD,
        ki = new User32.KEYBDINPUT()
        {
            # 设置虚拟键码
            wVk = (ushort)keyCode,
            # 计算扫描码
            wScan = (ushort)(User32.MapVirtualKey((uint)keyCode, 0) & 0xFFU),
            # 设置标志：扩展键标志（如果需要）和按键释放标志
            dwFlags = (isUseExtendedKey ? User32.KEYEVENTF.KEYEVENTF_EXTENDEDKEY : 0) | User32.KEYEVENTF.KEYEVENTF_KEYUP,
            time = 0,
            dwExtraInfo = IntPtr.Zero,
        },
    };
    # 将 INPUT 结构体实例赋值给 item 变量
    User32.INPUT item = input;
    # 将 item 添加到输入列表中
    _inputList.Add(item);
    # 返回当前对象实例
    return this;

    # 将指定键码的按键按下和释放事件添加到输入列表中
    public InputBuilder AddKeyPress(User32.VK keyCode, bool? isExtendedKey = null)
    {
        AddKeyDown(keyCode, isExtendedKey);
        AddKeyUp(keyCode, isExtendedKey);
        return this;
    }

    # 将一个字符的按下和释放事件添加到输入列表中
    public InputBuilder AddCharacter(char character)
    {
        # 创建按键按下事件的 INPUT 结构体实例
        User32.INPUT input = new()
        {
            type = User32.INPUTTYPE.INPUT_KEYBOARD,
            ki = new User32.KEYBDINPUT()
            {
                wVk = 0,
                wScan = character,
                dwFlags = User32.KEYEVENTF.KEYEVENTF_UNICODE,
                time = 0,
                dwExtraInfo = IntPtr.Zero,
            },
        };
        # 将按下事件添加到输入列表中
        User32.INPUT item = input;
        # 创建按键释放事件的 INPUT 结构体实例
        User32.INPUT input2 = new()
        {
            type = User32.INPUTTYPE.INPUT_KEYBOARD,
            ki = new User32.KEYBDINPUT()
            {
                wVk = 0,
                wScan = character,
                dwFlags = User32.KEYEVENTF.KEYEVENTF_KEYUP | User32.KEYEVENTF.KEYEVENTF_UNICODE,
                time = 0,
                dwExtraInfo = IntPtr.Zero,
            },
        };
        # 将释放事件添加到输入列表中
        User32.INPUT item2 = input2;
        # 如果字符符合特定条件，设置扩展键标志
        if ((character & '\u1234') == '\u1234')
        {
            item.ki = new User32.KEYBDINPUT()
            {
                wVk = item.ki.wVk,
                wScan = item.ki.wScan,
                dwFlags = item.ki.dwFlags | User32.KEYEVENTF.KEYEVENTF_EXTENDEDKEY,
                time = item.ki.time,
                dwExtraInfo = item.ki.dwExtraInfo,
            };
            item2.ki = new User32.KEYBDINPUT()
            {
                wVk = item2.ki.wVk,
                wScan = item2.ki.wScan,
                dwFlags = item2.ki.dwFlags | User32.KEYEVENTF.KEYEVENTF_EXTENDEDKEY,
                time = item2.ki.time,
                dwExtraInfo = item2.ki.dwExtraInfo,
            };
        }
        # 将带有扩展键标志的按下和释放事件添加到输入列表中
        _inputList.Add(item);
        _inputList.Add(item2);
        # 返回当前对象实例
        return this;
    }

    # 将多个字符的按下和释放事件逐一添加到输入列表中
    public InputBuilder AddCharacters(IEnumerable<char> characters)
    {
        foreach (char character in characters)
        {
            AddCharacter(character);
        }
        return this;
    }
    # 将字符字符串转换为字符数组并调用重载方法
        public InputBuilder AddCharacters(string characters)
        {
            # 调用重载方法，将字符字符串转换为字符数组
            return AddCharacters(characters.ToCharArray());
        }
    
        # 添加相对鼠标移动操作到输入列表
        public InputBuilder AddRelativeMouseMovement(int x, int y)
        {
            # 创建一个新的 INPUT 对象，设置为鼠标输入
            User32.INPUT item = new()
            {
                type = User32.INPUTTYPE.INPUT_MOUSE,
                mi = new User32.MOUSEINPUT()
                {
                    dx = x, # 设置 X 轴的相对移动距离
                    dy = y, # 设置 Y 轴的相对移动距离
                    dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_MOVE, # 指定鼠标移动事件
                },
            };
            # 将新创建的 INPUT 对象添加到输入列表
            _inputList.Add(item);
            # 返回当前实例以支持链式调用
            return this;
        }
    
        # 添加绝对鼠标移动操作到输入列表
        public InputBuilder AddAbsoluteMouseMovement(int absoluteX, int absoluteY)
        {
            # 创建一个新的 INPUT 对象，设置为鼠标输入
            User32.INPUT item = new()
            {
                type = User32.INPUTTYPE.INPUT_MOUSE,
                mi = new User32.MOUSEINPUT()
                {
                    dx = absoluteX, # 设置 X 轴的绝对坐标
                    dy = absoluteY, # 设置 Y 轴的绝对坐标
                    dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_MOVE | User32.MOUSEEVENTF.MOUSEEVENTF_ABSOLUTE, # 指定绝对移动事件
                },
            };
            # 将新创建的 INPUT 对象添加到输入列表
            _inputList.Add(item);
            # 返回当前实例以支持链式调用
            return this;
        }
    
        # 添加在虚拟桌面上的绝对鼠标移动操作到输入列表
        public InputBuilder AddAbsoluteMouseMovementOnVirtualDesktop(int absoluteX, int absoluteY)
        {
            # 创建一个新的 INPUT 对象，设置为鼠标输入
            User32.INPUT item = new()
            {
                type = User32.INPUTTYPE.INPUT_MOUSE,
                mi = new User32.MOUSEINPUT()
                {
                    dx = absoluteX, # 设置 X 轴的绝对坐标
                    dy = absoluteY, # 设置 Y 轴的绝对坐标
                    dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_MOVE | User32.MOUSEEVENTF.MOUSEEVENTF_ABSOLUTE | User32.MOUSEEVENTF.MOUSEEVENTF_VIRTUALDESK, # 指定绝对移动事件并包含虚拟桌面标志
                },
            };
            # 将新创建的 INPUT 对象添加到输入列表
            _inputList.Add(item);
            # 返回当前实例以支持链式调用
            return this;
        }
    
        # 添加鼠标按下操作到输入列表
        public InputBuilder AddMouseButtonDown(MouseButton button)
        {
            # 创建一个新的 INPUT 对象，设置为鼠标输入
            User32.INPUT item = new()
            {
                type = User32.INPUTTYPE.INPUT_MOUSE,
                mi = new User32.MOUSEINPUT()
                {
                    dwFlags = ToMouseButtonDownFlag(button), # 设置按下的鼠标按钮标志
                },
            };
            # 将新创建的 INPUT 对象添加到输入列表
            _inputList.Add(item);
            # 返回当前实例以支持链式调用
            return this;
        }
    
        # 添加鼠标 X 按钮按下操作到输入列表
        public InputBuilder AddMouseXButtonDown(int xButtonId)
        {
            # 创建一个新的 INPUT 对象，设置为鼠标输入
            User32.INPUT item = new()
            {
                type = User32.INPUTTYPE.INPUT_MOUSE,
                mi = new User32.MOUSEINPUT()
                {
                    dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_XDOWN, # 指定 X 按钮按下事件
                    mouseData = xButtonId, # 设置 X 按钮的 ID
                },
            };
            # 将新创建的 INPUT 对象添加到输入列表
            _inputList.Add(item);
            # 返回当前实例以支持链式调用
            return this;
        }
    
        # 添加鼠标按钮抬起操作到输入列表
        public InputBuilder AddMouseButtonUp(MouseButton button)
        {
            # 创建一个新的 INPUT 对象，设置为鼠标输入
            User32.INPUT item = new()
            {
                type = User32.INPUTTYPE.INPUT_MOUSE,
                mi = new User32.MOUSEINPUT()
                {
                    dwFlags = ToMouseButtonUpFlag(button), # 设置抬起的鼠标按钮标志
                },
            };
            # 将新创建的 INPUT 对象添加到输入列表
            _inputList.Add(item);
            # 返回当前实例以支持链式调用
            return this;
        }
    
        # 添加鼠标 X 按钮抬起操作到输入列表
        public InputBuilder AddMouseXButtonUp(int xButtonId)
    # 创建一个新的 User32.INPUT 实例，用于模拟鼠标事件
    {
        User32.INPUT item = new()
        {
            # 设置输入事件类型为鼠标事件
            type = User32.INPUTTYPE.INPUT_MOUSE,
            mi = new User32.MOUSEINPUT()
            {
                # 设置鼠标事件的标志为 MOUSEEVENTF_XUP（释放 X 按钮）
                dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_XUP,
                # 设置鼠标数据为 X 按钮 ID
                mouseData = xButtonId,
            },
        };
        # 将构建的输入事件添加到输入列表中
        _inputList.Add(item);
        # 返回当前的 InputBuilder 实例，允许链式调用
        return this;
    }

    # 添加一个鼠标按键点击事件（按下和释放）
    public InputBuilder AddMouseButtonClick(MouseButton button)
    {
        # 添加鼠标按下事件和鼠标释放事件
        return AddMouseButtonDown(button).AddMouseButtonUp(button);
    }

    # 添加一个鼠标 X 按钮点击事件（按下和释放）
    public InputBuilder AddMouseXButtonClick(int xButtonId)
    {
        # 添加 X 按钮按下事件和 X 按钮释放事件
        return AddMouseXButtonDown(xButtonId).AddMouseXButtonUp(xButtonId);
    }

    # 添加一个鼠标按键双击事件（点击两次）
    public InputBuilder AddMouseButtonDoubleClick(MouseButton button)
    {
        # 添加按键点击事件两次，即双击
        return AddMouseButtonClick(button).AddMouseButtonClick(button);
    }

    # 添加一个鼠标 X 按钮双击事件（点击两次）
    public InputBuilder AddMouseXButtonDoubleClick(int xButtonId)
    {
        # 添加 X 按钮点击事件两次，即双击
        return AddMouseXButtonClick(xButtonId).AddMouseXButtonClick(xButtonId);
    }

    # 添加垂直滚轮滚动事件
    public InputBuilder AddMouseVerticalWheelScroll(int scrollAmount)
    {
        # 创建一个新的 User32.INPUT 实例，用于模拟垂直滚轮滚动事件
        User32.INPUT item = new()
        {
            # 设置输入事件类型为鼠标事件
            type = User32.INPUTTYPE.INPUT_MOUSE,
            mi = new User32.MOUSEINPUT()
            {
                # 设置鼠标事件的标志为 MOUSEEVENTF_WHEEL（垂直滚轮滚动）
                dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_WHEEL,
                # 设置滚动量
                mouseData = scrollAmount,
            },
        };
        # 将构建的输入事件添加到输入列表中
        _inputList.Add(item);
        # 返回当前的 InputBuilder 实例，允许链式调用
        return this;
    }

    # 添加水平滚轮滚动事件
    public InputBuilder AddMouseHorizontalWheelScroll(int scrollAmount)
    {
        # 创建一个新的 User32.INPUT 实例，用于模拟水平滚轮滚动事件
        User32.INPUT item = new()
        {
            # 设置输入事件类型为鼠标事件
            type = User32.INPUTTYPE.INPUT_MOUSE,
            mi = new User32.MOUSEINPUT()
            {
                # 设置鼠标事件的标志为 MOUSEEVENTF_HWHEEL（水平滚轮滚动）
                dwFlags = User32.MOUSEEVENTF.MOUSEEVENTF_HWHEEL,
                # 设置滚动量
                mouseData = scrollAmount,
            },
        };
        # 将构建的输入事件添加到输入列表中
        _inputList.Add(item);
        # 返回当前的 InputBuilder 实例，允许链式调用
        return this;
    }

    # 将鼠标按键的按下事件标志转换为对应的 MOUSEEVENTF 标志
    private static User32.MOUSEEVENTF ToMouseButtonDownFlag(MouseButton button)
    {
        return button switch
        {
            # 左键按下
            MouseButton.LeftButton => User32.MOUSEEVENTF.MOUSEEVENTF_LEFTDOWN,
            # 中键按下
            MouseButton.MiddleButton => User32.MOUSEEVENTF.MOUSEEVENTF_MIDDLEDOWN,
            # 右键按下
            MouseButton.RightButton => User32.MOUSEEVENTF.MOUSEEVENTF_RIGHTDOWN,
            # 默认情况为左键按下
            _ => User32.MOUSEEVENTF.MOUSEEVENTF_LEFTDOWN,
        };
    }

    # 将鼠标按键的释放事件标志转换为对应的 MOUSEEVENTF 标志
    private static User32.MOUSEEVENTF ToMouseButtonUpFlag(MouseButton button)
    {
        return button switch
        {
            # 左键释放
            MouseButton.LeftButton => User32.MOUSEEVENTF.MOUSEEVENTF_LEFTUP,
            # 中键释放
            MouseButton.MiddleButton => User32.MOUSEEVENTF.MOUSEEVENTF_MIDDLEUP,
            # 右键释放
            MouseButton.RightButton => User32.MOUSEEVENTF.MOUSEEVENTF_RIGHTUP,
            # 默认情况为左键释放
            _ => User32.MOUSEEVENTF.MOUSEEVENTF_LEFTUP,
        };
    }

    # 输入列表，用于存储待处理的输入事件
    private readonly List<User32.INPUT> _inputList;
/// <summary>
/// https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes
/// </summary>
public enum VK2
{
    /// <summary>
    ///  ENTER key
    /// </summary>
    // 定义 VK_ENTER 枚举值，映射到 User32.VK.VK_RETURN 常量，表示 ENTER 键
    VK_ENTER = User32.VK.VK_RETURN,

    /// <summary>
    ///  The Unassigned code: The Num ENTER key.
    /// </summary>
    // 定义 VK_NUMPAD_ENTER 枚举值，使用 0x0E 作为标识，表示数字键盘上的 ENTER 键
    VK_NUMPAD_ENTER = 0x0E,
}
```