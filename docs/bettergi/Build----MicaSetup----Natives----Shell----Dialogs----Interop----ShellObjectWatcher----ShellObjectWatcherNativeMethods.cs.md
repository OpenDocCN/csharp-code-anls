# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\ShellObjectWatcher\ShellObjectWatcherNativeMethods.cs`

```cs
# 定义一个结构体 Message，包含与消息相关的数据
[StructLayout(LayoutKind.Sequential)]
public struct Message
{
    # 窗口句柄，消息的目标窗口
    private readonly nint windowHandle;
    # 消息的标识符
    private readonly uint msg;
    # 消息的附加参数
    private readonly nint wparam;
    # 消息的附加参数
    private readonly nint lparam;
    # 消息发生的时间
    private readonly int time;
    # 消息发生的位置
    private POINT point;

    # Message 结构体的构造函数
    internal Message(nint windowHandle, uint msg, nint wparam, nint lparam, int time, POINT point)
        : this()
    {
        # 初始化结构体字段
        this.windowHandle = windowHandle;
        this.msg = msg;
        this.wparam = wparam;
        this.lparam = lparam;
        this.time = time;
        this.point = point;
    }

    # 只读属性，获取 lparam 的值
    public nint LParam => lparam;

    # 只读属性，获取 msg 的值
    public uint Msg => msg;

    # 只读属性，获取 point 的值
    public POINT Point => point;

    # 只读属性，获取 time 的值
    public int Time => time;

    # 只读属性，获取 windowHandle 的值
    public nint WindowHandle => windowHandle;

    # 只读属性，获取 wparam 的值
    public nint WParam => wparam;

    # 重载 != 运算符，用于比较两个 Message 结构体是否不相等
    public static bool operator !=(Message first, Message second) => !(first == second);

    # 重载 == 运算符，用于比较两个 Message 结构体是否相等
    public static bool operator ==(Message first, Message second) => first.WindowHandle == second.WindowHandle
            && first.Msg == second.Msg
            && first.WParam == second.WParam
            && first.LParam == second.LParam
            && first.Time == second.Time
            && first.Point == second.Point;

    # 重写 Equals 方法，用于比较 Message 结构体的相等性
    public override bool Equals(object obj) => (obj != null && obj is Message) ? this == (Message)obj : false;

    # 重写 GetHashCode 方法，生成 Message 结构体的哈希码
    public override int GetHashCode()
    {
        var hash = WindowHandle.GetHashCode();
        hash = hash * 31 + Msg.GetHashCode();
        hash = hash * 31 + WParam.GetHashCode();
        hash = hash * 31 + LParam.GetHashCode();
        hash = hash * 31 + Time.GetHashCode();
        hash = hash * 31 + Point.GetHashCode();
        return hash;
    }
}

# 定义一个结构体 WindowClassEx，包含窗口类的扩展信息
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
internal struct WindowClassEx
{
    # 结构体的大小
    internal uint Size;
    # 窗口类的样式
    internal uint Style;

    # 窗口处理函数的委托
    internal ShellObjectWatcherNativeMethods.WndProcDelegate WndProc;

    # 额外的类字节
    internal int ExtraClassBytes;
    # 额外的窗口字节
    internal int ExtraWindowBytes;
    # 实例句柄
    internal nint InstanceHandle;
    # 图标句柄
    internal nint IconHandle;
    # 光标句柄
    internal nint CursorHandle;
    # 背景画刷句柄
    internal nint BackgroundBrushHandle;

    # 菜单名称
    internal string MenuName;
    # 窗口类名称
    internal string ClassName;

    # 小图标句柄
    internal nint SmallIconHandle;
}

# 定义一个包含本机方法的静态类 ShellObjectWatcherNativeMethods
internal static class ShellObjectWatcherNativeMethods
{
    # 定义一个窗口处理函数的委托
    public delegate int WndProcDelegate(nint hwnd, uint msg, nint wparam, nint lparam);

    # 导入 OLE32 库中的 CreateBindCtx 方法
    [DllImport(Lib.Ole32)]
    public static extern HResult CreateBindCtx(
        int reserved,
        [Out] out IBindCtx bindCtx);

    # 导入 User32 库中的方法，字符集为 Unicode，设置了错误信息
    [DllImport(Lib.User32, CharSet = CharSet.Unicode, SetLastError = true)]
    # 声明一个外部方法用于创建扩展样式窗口
    public static extern nint CreateWindowEx(
        int extendedStyle,  # 窗口的扩展样式
        [MarshalAs(UnmanagedType.LPWStr)]
        string className,  # 窗口类名
        [MarshalAs(UnmanagedType.LPWStr)]
        string windowName,  # 窗口名称
        int style,  # 窗口样式
        int x,  # 窗口的 X 坐标
        int y,  # 窗口的 Y 坐标
        int width,  # 窗口的宽度
        int height,  # 窗口的高度
        nint parentHandle,  # 父窗口的句柄
        nint menuHandle,  # 菜单句柄
        nint instanceHandle,  # 实例句柄
        nint additionalData);  # 额外数据

    # 声明一个外部方法用于处理窗口消息的默认处理过程
    [DllImport(Lib.User32)]
    public static extern int DefWindowProc(
        nint hwnd,  # 窗口的句柄
        uint msg,  # 消息类型
        nint wparam,  # 消息的参数
        nint lparam);  # 消息的附加参数

    # 声明一个外部方法用于分发消息到窗口过程
    [DllImport(Lib.User32)]
    public static extern void DispatchMessage([In] ref Message message);  # 消息结构的引用

    # 声明一个外部方法用于从消息队列中检索消息
    [DllImport(Lib.User32)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool GetMessage(
        [Out] out Message message,  # 用于存储消息的结构
        nint windowHandle,  # 窗口的句柄
        uint filterMinMessage,  # 最小消息过滤器
        uint filterMaxMessage);  # 最大消息过滤器

    # 声明一个外部方法用于注册窗口类
    [DllImport(Lib.User32, CharSet = CharSet.Unicode, SetLastError = true)]
    public static extern uint RegisterClassEx(
        ref WindowClassEx windowClass  # 窗口类的描述信息
        );
# 结束一个代码块或代码块的闭合
}
```