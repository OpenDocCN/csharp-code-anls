# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardKeys.cs`

```cs
﻿namespace Fischless.KeyboardCapture;

/// <summary>
/// https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes
/// But cutmized 0x0E as NumEnter.
/// </summary>
[Flags]
public enum KeyboardKeys
{
    /// <summary>
    ///  The bit mask to extract a key code from a key value.
    /// </summary>
    KeyCode = 0x0000FFFF,

    /// <summary>
    ///  The bit mask to extract modifiers from a key value.
    /// </summary>
    Modifiers = unchecked((int)0xFFFF0000),

    /// <summary>
    ///  No key pressed.
    /// </summary>
    None = 0x00,

    /// <summary>
    ///  The left mouse button.
    /// </summary>
    LButton = 0x01,

    /// <summary>
    ///  The right mouse button.
    /// </summary>
    RButton = 0x02,

    /// <summary>
    ///  The CANCEL key.
    /// </summary>
    Cancel = 0x03,

    /// <summary>
    ///  The middle mouse button (three-button mouse).
    /// </summary>
    MButton = 0x04,

    /// <summary>
    ///  The first x mouse button (five-button mouse).
    /// </summary>
    XButton1 = 0x05,

    /// <summary>
    ///  The second x mouse button (five-button mouse).
    /// </summary>
    XButton2 = 0x06,

    /// <summary>
    ///  The BACKSPACE key.
    /// </summary>
    Back = 0x08,

    /// <summary>
    ///  The TAB key.
    /// </summary>
    Tab = 0x09,

    /// <summary>
    ///  The CLEAR key.
    /// </summary>
    LineFeed = 0x0A,

    /// <summary>
    ///  The CLEAR key.
    /// </summary>
    Clear = 0x0C,

    /// <summary>
    ///  The RETURN key.
    /// </summary>
    Return = 0x0D,

    /// <summary>
    ///  The ENTER key.
    /// </summary>
    Enter = Return,

    /// <summary>
    ///  The Unassigned code: The Num ENTER key.
    /// </summary>
    NumEnter = 0x0E,

    /// <summary>
    ///  The SHIFT key.
    /// </summary>
    ShiftKey = 0x10,

    /// <summary>
    ///  The CTRL key.
    /// </summary>
    ControlKey = 0x11,

    /// <summary>
    ///  The ALT key.
    /// </summary>
    Menu = 0x12,

    /// <summary>
    ///  The PAUSE key.
    /// </summary>
    Pause = 0x13,

    /// <summary>
    ///  The CAPS LOCK key.
    /// </summary>
    Capital = 0x14,

    /// <summary>
    ///  The CAPS LOCK key.
    /// </summary>
    CapsLock = 0x14,

    /// <summary>
    ///  The IME Kana mode key.
    /// </summary>
    KanaMode = 0x15,

    /// <summary>
    ///  The IME Hanguel mode key.
    /// </summary>
    HanguelMode = 0x15,

    /// <summary>
    ///  The IME Hangul mode key.
    /// </summary>
    HangulMode = 0x15,

    /// <summary>
    ///  The IME Junja mode key.
    /// </summary>
    JunjaMode = 0x17,

    /// <summary>
    ///  The IME Final mode key.
    /// </summary>
    FinalMode = 0x18,

    /// <summary>
    ///  The IME Hanja mode key.
    /// </summary>
    HanjaMode = 0x19,

    /// <summary>
    ///  The IME Kanji mode key.
    /// </summary>
    KanjiMode = 0x19,

    /// <summary>
    ///  The ESC key.
    /// </summary>
    Escape = 0x1B,
    /// <summary>
    ///  The IME Convert key.
    /// </summary>
    # IME 转换键的虚拟键码
    IMEConvert = 0x1C,

    /// <summary>
    ///  The IME NonConvert key.
    /// </summary>
    # IME 不转换键的虚拟键码
    IMENonconvert = 0x1D,

    /// <summary>
    ///  The IME Accept key.
    /// </summary>
    # IME 接受键的虚拟键码
    IMEAccept = 0x1E,

    /// <summary>
    ///  The IME Accept key.
    /// </summary>
    # IME 接受键的虚拟键码，定义为与 IMEAccept 相同
    IMEAceept = IMEAccept,

    /// <summary>
    ///  The IME Mode change request.
    /// </summary>
    # IME 模式更改请求的虚拟键码
    IMEModeChange = 0x1F,

    /// <summary>
    ///  The SPACEBAR key.
    /// </summary>
    # 空格键的虚拟键码
    Space = 0x20,

    /// <summary>
    ///  The PAGE UP key.
    /// </summary>
    # Page Up 键的虚拟键码
    Prior = 0x21,

    /// <summary>
    ///  The PAGE UP key.
    /// </summary>
    # Page Up 键的虚拟键码，定义为与 Prior 相同
    PageUp = Prior,

    /// <summary>
    ///  The PAGE DOWN key.
    /// </summary>
    # Page Down 键的虚拟键码
    Next = 0x22,

    /// <summary>
    ///  The PAGE DOWN key.
    /// </summary>
    # Page Down 键的虚拟键码，定义为与 Next 相同
    PageDown = Next,

    /// <summary>
    ///  The END key.
    /// </summary>
    # End 键的虚拟键码
    End = 0x23,

    /// <summary>
    ///  The HOME key.
    /// </summary>
    # Home 键的虚拟键码
    Home = 0x24,

    /// <summary>
    ///  The LEFT ARROW key.
    /// </summary>
    # 左箭头键的虚拟键码
    Left = 0x25,

    /// <summary>
    ///  The UP ARROW key.
    /// </summary>
    # 上箭头键的虚拟键码
    Up = 0x26,

    /// <summary>
    ///  The RIGHT ARROW key.
    /// </summary>
    # 右箭头键的虚拟键码
    Right = 0x27,

    /// <summary>
    ///  The DOWN ARROW key.
    /// </summary>
    # 下箭头键的虚拟键码
    Down = 0x28,

    /// <summary>
    ///  The SELECT key.
    /// </summary>
    # 选择键的虚拟键码
    Select = 0x29,

    /// <summary>
    ///  The PRINT key.
    /// </summary>
    # 打印键的虚拟键码
    Print = 0x2A,

    /// <summary>
    ///  The EXECUTE key.
    /// </summary>
    # 执行键的虚拟键码
    Execute = 0x2B,

    /// <summary>
    ///  The PRINT SCREEN key.
    /// </summary>
    # 打印屏幕键的虚拟键码
    Snapshot = 0x2C,

    /// <summary>
    ///  The PRINT SCREEN key.
    /// </summary>
    # 打印屏幕键的虚拟键码，定义为与 Snapshot 相同
    PrintScreen = Snapshot,

    /// <summary>
    ///  The INS key.
    /// </summary>
    # 插入键的虚拟键码
    Insert = 0x2D,

    /// <summary>
    ///  The DEL key.
    /// </summary>
    # 删除键的虚拟键码
    Delete = 0x2E,

    /// <summary>
    ///  The HELP key.
    /// </summary>
    # 帮助键的虚拟键码
    Help = 0x2F,

    /// <summary>
    ///  The 0 key.
    /// </summary>
    # 数字键 0 的虚拟键码
    D0 = 0x30, // 0

    /// <summary>
    ///  The 1 key.
    /// </summary>
    # 数字键 1 的虚拟键码
    D1 = 0x31, // 1

    /// <summary>
    ///  The 2 key.
    /// </summary>
    # 数字键 2 的虚拟键码
    D2 = 0x32, // 2

    /// <summary>
    ///  The 3 key.
    /// </summary>
    # 数字键 3 的虚拟键码
    D3 = 0x33, // 3

    /// <summary>
    ///  The 4 key.
    /// </summary>
    # 数字键 4 的虚拟键码
    D4 = 0x34, // 4

    /// <summary>
    ///  The 5 key.
    /// </summary>
    # 数字键 5 的虚拟键码
    D5 = 0x35, // 5

    /// <summary>
    ///  The 6 key.
    /// </summary>
    # 数字键 6 的虚拟键码
    D6 = 0x36, // 6

    /// <summary>
    ///  The 7 key.
    /// </summary>
    # 数字键 7 的虚拟键码
    D7 = 0x37, // 7

    /// <summary>
    ///  The 8 key.
    /// </summary>
    # 数字键 8 的虚拟键码
    D8 = 0x38, // 8

    /// <summary>
    ///  The 9 key.
    /// </summary>
    # 数字键 9 的虚拟键码
    D9 = 0x39, // 9

    /// <summary>
    ///  The A key.
    /// </summary>
    # 字母键 A 的虚拟键码
    A = 0x41,

    /// <summary>
    ///  The B key.
    /// </summary>
    # 字母键 B 的虚拟键码
    B = 0x42,

    /// <summary>
    ///  The C key.
    /// </summary>
    # The key code for the 'C' key
    C = 0x43,

    /// <summary>
    ///  The D key.
    /// </summary>
    # The key code for the 'D' key
    D = 0x44,

    /// <summary>
    ///  The E key.
    /// </summary>
    # The key code for the 'E' key
    E = 0x45,

    /// <summary>
    ///  The F key.
    /// </summary>
    # The key code for the 'F' key
    F = 0x46,

    /// <summary>
    ///  The G key.
    /// </summary>
    # The key code for the 'G' key
    G = 0x47,

    /// <summary>
    ///  The H key.
    /// </summary>
    # The key code for the 'H' key
    H = 0x48,

    /// <summary>
    ///  The I key.
    /// </summary>
    # The key code for the 'I' key
    I = 0x49,

    /// <summary>
    ///  The J key.
    /// </summary>
    # The key code for the 'J' key
    J = 0x4A,

    /// <summary>
    ///  The K key.
    /// </summary>
    # The key code for the 'K' key
    K = 0x4B,

    /// <summary>
    ///  The L key.
    /// </summary>
    # The key code for the 'L' key
    L = 0x4C,

    /// <summary>
    ///  The M key.
    /// </summary>
    # The key code for the 'M' key
    M = 0x4D,

    /// <summary>
    ///  The N key.
    /// </summary>
    # The key code for the 'N' key
    N = 0x4E,

    /// <summary>
    ///  The O key.
    /// </summary>
    # The key code for the 'O' key
    O = 0x4F,

    /// <summary>
    ///  The P key.
    /// </summary>
    # The key code for the 'P' key
    P = 0x50,

    /// <summary>
    ///  The Q key.
    /// </summary>
    # The key code for the 'Q' key
    Q = 0x51,

    /// <summary>
    ///  The R key.
    /// </summary>
    # The key code for the 'R' key
    R = 0x52,

    /// <summary>
    ///  The S key.
    /// </summary>
    # The key code for the 'S' key
    S = 0x53,

    /// <summary>
    ///  The T key.
    /// </summary>
    # The key code for the 'T' key
    T = 0x54,

    /// <summary>
    ///  The U key.
    /// </summary>
    # The key code for the 'U' key
    U = 0x55,

    /// <summary>
    ///  The V key.
    /// </summary>
    # The key code for the 'V' key
    V = 0x56,

    /// <summary>
    ///  The W key.
    /// </summary>
    # The key code for the 'W' key
    W = 0x57,

    /// <summary>
    ///  The X key.
    /// </summary>
    # The key code for the 'X' key
    X = 0x58,

    /// <summary>
    ///  The Y key.
    /// </summary>
    # The key code for the 'Y' key
    Y = 0x59,

    /// <summary>
    ///  The Z key.
    /// </summary>
    # The key code for the 'Z' key
    Z = 0x5A,

    /// <summary>
    ///  The left Windows logo key (Microsoft Natural Keyboard).
    /// </summary>
    # The key code for the left Windows logo key
    LWin = 0x5B,

    /// <summary>
    ///  The right Windows logo key (Microsoft Natural Keyboard).
    /// </summary>
    # The key code for the right Windows logo key
    RWin = 0x5C,

    /// <summary>
    ///  The Application key (Microsoft Natural Keyboard).
    /// </summary>
    # The key code for the Application key
    Apps = 0x5D,

    /// <summary>
    ///  The Computer Sleep key.
    /// </summary>
    # The key code for the Computer Sleep key
    Sleep = 0x5F,

    /// <summary>
    ///  The 0 key on the numeric keypad.
    /// </summary>
    # The key code for '0' on the numeric keypad
    NumPad0 = 0x60,

    /// <summary>
    ///  The 1 key on the numeric keypad.
    /// </summary>
    # The key code for '1' on the numeric keypad
    NumPad1 = 0x61,

    /// <summary>
    ///  The 2 key on the numeric keypad.
    /// </summary>
    # The key code for '2' on the numeric keypad
    NumPad2 = 0x62,

    /// <summary>
    ///  The 3 key on the numeric keypad.
    /// </summary>
    # The key code for '3' on the numeric keypad
    NumPad3 = 0x63,

    /// <summary>
    ///  The 4 key on the numeric keypad.
    /// </summary>
    # The key code for '4' on the numeric keypad
    NumPad4 = 0x64,

    /// <summary>
    ///  The 5 key on the numeric keypad.
    /// </summary>
    # The key code for '5' on the numeric keypad
    NumPad5 = 0x65,

    /// <summary>
    ///  The 6 key on the numeric keypad.
    /// </summary>
    # The key code for '6' on the numeric keypad
    NumPad6 = 0x66,

    /// <summary>
    ///  The 7 key on the numeric keypad.
    /// </summary>
    # The key code for '7' on the numeric keypad
    NumPad7 = 0x67,

    /// <summary>
    /// <summary>
    ///  The 8 key on the numeric keypad.
    /// </summary>
    NumPad8 = 0x68,  # 数字键盘上的 8 键

    /// <summary>
    ///  The 9 key on the numeric keypad.
    /// </summary>
    NumPad9 = 0x69,  # 数字键盘上的 9 键

    /// <summary>
    ///  The Multiply key.
    /// </summary>
    Multiply = 0x6A,  # 乘法键

    /// <summary>
    ///  The Add key.
    /// </summary>
    Add = 0x6B,  # 加法键

    /// <summary>
    ///  The Separator key.
    /// </summary>
    Separator = 0x6C,  # 分隔键

    /// <summary>
    ///  The Subtract key.
    /// </summary>
    Subtract = 0x6D,  # 减法键

    /// <summary>
    ///  The Decimal key.
    /// </summary>
    Decimal = 0x6E,  # 小数点键

    /// <summary>
    ///  The Divide key.
    /// </summary>
    Divide = 0x6F,  # 除法键

    /// <summary>
    ///  The F1 key.
    /// </summary>
    F1 = 0x70,  # F1 功能键

    /// <summary>
    ///  The F2 key.
    /// </summary>
    F2 = 0x71,  # F2 功能键

    /// <summary>
    ///  The F3 key.
    /// </summary>
    F3 = 0x72,  # F3 功能键

    /// <summary>
    ///  The F4 key.
    /// </summary>
    F4 = 0x73,  # F4 功能键

    /// <summary>
    ///  The F5 key.
    /// </summary>
    F5 = 0x74,  # F5 功能键

    /// <summary>
    ///  The F6 key.
    /// </summary>
    F6 = 0x75,  # F6 功能键

    /// <summary>
    ///  The F7 key.
    /// </summary>
    F7 = 0x76,  # F7 功能键

    /// <summary>
    ///  The F8 key.
    /// </summary>
    F8 = 0x77,  # F8 功能键

    /// <summary>
    ///  The F9 key.
    /// </summary>
    F9 = 0x78,  # F9 功能键

    /// <summary>
    ///  The F10 key.
    /// </summary>
    F10 = 0x79,  # F10 功能键

    /// <summary>
    ///  The F11 key.
    /// </summary>
    F11 = 0x7A,  # F11 功能键

    /// <summary>
    ///  The F12 key.
    /// </summary>
    F12 = 0x7B,  # F12 功能键

    /// <summary>
    ///  The F13 key.
    /// </summary>
    F13 = 0x7C,  # F13 功能键

    /// <summary>
    ///  The F14 key.
    /// </summary>
    F14 = 0x7D,  # F14 功能键

    /// <summary>
    ///  The F15 key.
    /// </summary>
    F15 = 0x7E,  # F15 功能键

    /// <summary>
    ///  The F16 key.
    /// </summary>
    F16 = 0x7F,  # F16 功能键

    /// <summary>
    ///  The F17 key.
    /// </summary>
    F17 = 0x80,  # F17 功能键

    /// <summary>
    ///  The F18 key.
    /// </summary>
    F18 = 0x81,  # F18 功能键

    /// <summary>
    ///  The F19 key.
    /// </summary>
    F19 = 0x82,  # F19 功能键

    /// <summary>
    ///  The F20 key.
    /// </summary>
    F20 = 0x83,  # F20 功能键

    /// <summary>
    ///  The F21 key.
    /// </summary>
    F21 = 0x84,  # F21 功能键

    /// <summary>
    ///  The F22 key.
    /// </summary>
    F22 = 0x85,  # F22 功能键

    /// <summary>
    ///  The F23 key.
    /// </summary>
    F23 = 0x86,  # F23 功能键

    /// <summary>
    ///  The F24 key.
    /// </summary>
    F24 = 0x87,  # F24 功能键

    /// <summary>
    ///  The NUM LOCK key.
    /// </summary>
    NumLock = 0x90,  # 数字锁定键

    /// <summary>
    ///  The SCROLL LOCK key.
    /// </summary>
    Scroll = 0x91,  # 滚动锁定键

    /// <summary>
    ///  The left SHIFT key.
    /// </summary>
    LShiftKey = 0xA0,  # 左 SHIFT 键

    /// <summary>
    ///  The right SHIFT key.
    /// </summary>
    RShiftKey = 0xA1,  # 右 SHIFT 键

    /// <summary>
    ///  The left CTRL key.
    /// </summary>
    LControlKey = 0xA2,  # 左 CTRL 键

    /// <summary>
    ///  The right CTRL key.
    /// </summary>
    # 定义右侧控制键的虚拟键码
    RControlKey = 0xA3,

    # 定义左侧 ALT 键的虚拟键码
    LMenu = 0xA4,

    # 定义右侧 ALT 键的虚拟键码
    RMenu = 0xA5,

    # 定义浏览器返回键的虚拟键码
    BrowserBack = 0xA6,

    # 定义浏览器前进键的虚拟键码
    BrowserForward = 0xA7,

    # 定义浏览器刷新键的虚拟键码
    BrowserRefresh = 0xA8,

    # 定义浏览器停止键的虚拟键码
    BrowserStop = 0xA9,

    # 定义浏览器搜索键的虚拟键码
    BrowserSearch = 0xAA,

    # 定义浏览器收藏夹键的虚拟键码
    BrowserFavorites = 0xAB,

    # 定义浏览器主页键的虚拟键码
    BrowserHome = 0xAC,

    # 定义音量静音键的虚拟键码
    VolumeMute = 0xAD,

    # 定义音量减小键的虚拟键码
    VolumeDown = 0xAE,

    # 定义音量增大键的虚拟键码
    VolumeUp = 0xAF,

    # 定义媒体下一曲键的虚拟键码
    MediaNextTrack = 0xB0,

    # 定义媒体上一曲键的虚拟键码
    MediaPreviousTrack = 0xB1,

    # 定义媒体停止键的虚拟键码
    MediaStop = 0xB2,

    # 定义媒体播放/暂停键的虚拟键码
    MediaPlayPause = 0xB3,

    # 定义启动邮件客户端键的虚拟键码
    LaunchMail = 0xB4,

    # 定义选择媒体键的虚拟键码
    SelectMedia = 0xB5,

    # 定义启动应用程序1键的虚拟键码
    LaunchApplication1 = 0xB6,

    # 定义启动应用程序2键的虚拟键码
    LaunchApplication2 = 0xB7,

    # 定义 OEM 分号键的虚拟键码
    OemSemicolon = 0xBA,

    # 定义 OEM 1 键的虚拟键码（与 OEM 分号键相同）
    Oem1 = OemSemicolon,

    # 定义 OEM 加号键的虚拟键码
    Oemplus = 0xBB,

    # 定义 OEM 逗号键的虚拟键码
    Oemcomma = 0xBC,

    # 定义 OEM 减号键的虚拟键码
    OemMinus = 0xBD,

    # 定义 OEM 句点键的虚拟键码
    OemPeriod = 0xBE,

    # 定义 OEM 问号键的虚拟键码
    OemQuestion = 0xBF,

    # 定义 OEM 2 键的虚拟键码（与 OEM 问号键相同）
    Oem2 = OemQuestion,

    # 定义 OEM 波浪键的虚拟键码
    Oemtilde = 0xC0,

    # 定义 OEM 3 键的虚拟键码（与 OEM 波浪键相同）
    Oem3 = Oemtilde,

    # 定义 OEM 开括号键的虚拟键码
    OemOpenBrackets = 0xDB,

    # 定义 OEM 4 键的虚拟键码
    # 将 OemOpenBrackets 的值赋给 Oem4
    Oem4 = OemOpenBrackets,

    # <summary>
    #  Oem Pipe 键。
    # </summary>
    # 将 0xDC 赋值给 OemPipe
    OemPipe = 0xDC,

    # <summary>
    #  Oem 5 键。
    # </summary>
    # 将 OemPipe 的值赋给 Oem5
    Oem5 = OemPipe,

    # <summary>
    #  Oem Close Brackets 键。
    # </summary>
    # 将 0xDD 赋值给 OemCloseBrackets
    OemCloseBrackets = 0xDD,

    # <summary>
    #  Oem 6 键。
    # </summary>
    # 将 OemCloseBrackets 的值赋给 Oem6
    Oem6 = OemCloseBrackets,

    # <summary>
    #  Oem Quotes 键。
    # </summary>
    # 将 0xDE 赋值给 OemQuotes
    OemQuotes = 0xDE,

    # <summary>
    #  Oem 7 键。
    # </summary>
    # 将 OemQuotes 的值赋给 Oem7
    Oem7 = OemQuotes,

    # <summary>
    #  Oem8 键。
    # </summary>
    # 将 0xDF 赋值给 Oem8
    Oem8 = 0xDF,

    # <summary>
    #  Oem Backslash 键。
    # </summary>
    # 将 0xE2 赋值给 OemBackslash
    OemBackslash = 0xE2,

    # <summary>
    #  Oem 102 键。
    # </summary>
    # 将 OemBackslash 的值赋给 Oem102
    Oem102 = OemBackslash,

    # <summary>
    #  PROCESS KEY 键。
    # </summary>
    # 将 0xE5 赋值给 ProcessKey
    ProcessKey = 0xE5,

    # <summary>
    #  Packet KEY 键。
    # </summary>
    # 将 0xE7 赋值给 Packet
    Packet = 0xE7,

    # <summary>
    #  ATTN 键。
    # </summary>
    # 将 0xF6 赋值给 Attn
    Attn = 0xF6,

    # <summary>
    #  CRSEL 键。
    # </summary>
    # 将 0xF7 赋值给 Crsel
    Crsel = 0xF7,

    # <summary>
    #  EXSEL 键。
    # </summary>
    # 将 0xF8 赋值给 Exsel
    Exsel = 0xF8,

    # <summary>
    #  ERASE EOF 键。
    # </summary>
    # 将 0xF9 赋值给 EraseEof
    EraseEof = 0xF9,

    # <summary>
    #  PLAY 键。
    # </summary>
    # 将 0xFA 赋值给 Play
    Play = 0xFA,

    # <summary>
    #  ZOOM 键。
    # </summary>
    # 将 0xFB 赋值给 Zoom
    Zoom = 0xFB,

    # <summary>
    #  预留常量，用于未来用途。
    # </summary>
    # 将 0xFC 赋值给 NoName
    NoName = 0xFC,

    # <summary>
    #  PA1 键。
    # </summary>
    # 将 0xFD 赋值给 Pa1
    Pa1 = 0xFD,

    # <summary>
    #  CLEAR 键。
    # </summary>
    # 将 0xFE 赋值给 OemClear
    OemClear = 0xFE,

    # <summary>
    #  SHIFT 修饰键。
    # </summary>
    # 将 0x00010000 赋值给 Shift
    Shift = 0x00010000,

    # <summary>
    #  CTRL 修饰键。
    # </summary>
    # 将 0x00020000 赋值给 Control
    Control = 0x00020000,

    # <summary>
    #  ALT 修饰键。
    # </summary>
    # 将 0x00040000 赋值给 Alt
    Alt = 0x00040000,
你提供的代码只有一个 `}` 符号，似乎是一个不完整的代码片段。请提供完整的代码行或者上下文，以便我能为每一行添加适当的注释。
```