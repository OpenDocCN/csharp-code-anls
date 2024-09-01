# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\ShellExtensions\HandlerNativeMethods.cs`

```cs
# 定义 IInitializeWithFile 接口，用于初始化文件
[ComImport]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
[Guid("b7d14566-0509-4cce-a71f-0a554233bd9b")]
public interface IInitializeWithFile
{
    # 初始化方法，接受文件路径和访问模式作为参数
    void Initialize([MarshalAs(UnmanagedType.LPWStr)] string filePath, AccessModes fileMode);
}

# 定义 IInitializeWithItem 接口，用于初始化 Shell 项
[ComImport]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
[Guid("7f73be3f-fb79-493c-a6c7-7ee14e245841")]
public interface IInitializeWithItem
{
    # 初始化方法，接受 Shell 项和访问模式作为参数
    void Initialize([In, MarshalAs(UnmanagedType.IUnknown)] object shellItem, AccessModes accessMode);
}

# 定义 IInitializeWithStream 接口，用于初始化流
[SuppressMessage("Microsoft.Naming", "CA1711:IdentifiersShouldNotHaveIncorrectSuffix")]
[ComVisible(true)]
[Guid("b824b49d-22ac-4161-ac8a-9916e8fa3f7f")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithStream
{
    # 初始化方法，接受 IStream 对象和访问模式作为参数
    void Initialize(IStream stream, AccessModes fileMode);
}

# 定义 IThumbnailProvider 接口，用于提供缩略图
[ComVisible(true)]
[Guid("e357fccd-a995-4576-b01f-234630154e96")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IThumbnailProvider
{
    # 获取缩略图的方法，返回位图句柄和位图类型
    [SuppressMessage("Microsoft.Design", "CA1021:AvoidOutParameters", MessageId = "2#"), System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Design", "CA1021:AvoidOutParameters", MessageId = "1#")]
    void GetThumbnail(uint squareLength, out nint bitmapHandle, out uint bitmapType);
}

# 定义 IObjectWithSite 接口，用于设置和获取站点对象
[ComImport]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
[Guid("fc4801a3-2ba9-11cf-a229-00aa003d7352")]
internal interface IObjectWithSite
{
    # 设置站点对象
    void SetSite([In, MarshalAs(UnmanagedType.IUnknown)] object pUnkSite);

    # 获取站点对象
    void GetSite(ref Guid riid, [MarshalAs(UnmanagedType.IUnknown)] out object ppvSite);
}

# 定义 IOleWindow 接口，用于获取窗口句柄和上下文敏感帮助
[ComImport]
[Guid("00000114-0000-0000-C000-000000000046")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IOleWindow
{
    # 获取窗口句柄
    void GetWindow(out nint phwnd);

    # 上下文敏感帮助
    void ContextSensitiveHelp([MarshalAs(UnmanagedType.Bool)] bool fEnterMode);
}

# 定义 IPreviewHandler 接口，用于处理预览窗口
[ComImport]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
[Guid("8895b1c6-b41f-4c1c-a562-0d564250836f")]
internal interface IPreviewHandler
{
    # 设置窗口和矩形区域
    void SetWindow(nint hwnd, ref RECT rect);

    # 设置矩形区域
    void SetRect(ref RECT rect);

    # 执行预览
    void DoPreview();

    # 卸载预览
    void Unload();

    # 设置焦点
    void SetFocus();

    # 查询焦点
    void QueryFocus(out nint phwnd);

    # 处理加速器
    [PreserveSig]
    HResult TranslateAccelerator(ref MSG pmsg);
}

# 定义 IPreviewHandlerFrame 接口，用于处理预览处理程序帧
[ComImport]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
[Guid("fec87aaf-35f9-447a-adb7-20234491401a")]
internal interface IPreviewHandlerFrame
{
    # 获取窗口上下文
    void GetWindowContext(nint pinfo);

    # 处理加速器
    [PreserveSig]
    HResult TranslateAccelerator(ref MSG pmsg);
};

# 定义 IPreviewHandlerVisuals 接口，用于设置预览视觉效果
[ComImport]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
[Guid("8327b13c-b63f-4b24-9b8a-d010dcc3f599")]
internal interface IPreviewHandlerVisuals
{
    # 设置背景颜色
    void SetBackgroundColor(COLORREF color);

    # 设置字体
    void SetFont(ref LogFont plf);
    // 设置文本颜色为指定的 COLORREF 值
    void SetTextColor(COLORREF color);
# 结束之前的代码块
}

# 定义 COLORREF 结构体，表示颜色的一个四字节值
[StructLayout(LayoutKind.Sequential)]
internal struct COLORREF
{
    # 颜色的四字节无符号整数
    public uint Dword;

    # 将四字节颜色值转换为 Color 对象
    public Color Color => Color.FromArgb(
                (int)(0x000000FFU & Dword), # 提取蓝色分量
                (int)(0x0000FF00U & Dword) >> 8, # 提取绿色分量
                (int)(0x00FF0000U & Dword) >> 16); # 提取红色分量
}

# 定义 MSG 结构体，表示 Windows 消息
[StructLayout(LayoutKind.Sequential)]
internal struct MSG
{
    # 消息的窗口句柄
    public nint hwnd;
    # 消息的标识符
    public int message;
    # 消息的附加参数1
    public nint wParam;
    # 消息的附加参数2
    public nint lParam;
    # 消息发生的时间
    public int time;
    # 消息发生时的 X 坐标
    public int pt_x;
    # 消息发生时的 Y 坐标
    public int pt_y;
}

# 定义 LogFont 类，表示字体的各种属性
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
internal class LogFont
{
    # 字体的高度
    internal int height;
    # 字体的宽度
    internal int width;
    # 字体的倾斜角度
    internal int escapement;
    # 字体的方向
    internal int orientation;
    # 字体的粗细
    internal int weight;
    # 是否为斜体
    internal byte italic;
    # 是否有下划线
    internal byte underline;
    # 是否有删除线
    internal byte strikeOut;
    # 字符集
    internal byte charSet;
    # 输出精度
    internal byte outPrecision;
    # 剪裁精度
    internal byte clipPrecision;
    # 字体质量
    internal byte quality;
    # 字体的排印和家族
    internal byte pitchAndFamily;

    # 字体名称，固定长度的字符串
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 32)]
    internal string lfFaceName = string.Empty;
}
```