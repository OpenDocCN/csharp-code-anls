# `.\better-genshin-impact\Build\MicaSetup\Natives\User32.cs`

```cs
# 引入必要的系统库和类型
﻿using System;
using System.Runtime.InteropServices;
using System.Security;
using System.Text;

namespace MicaSetup.Natives;

# 定义一个静态类 User32 用于存放 Windows API 函数的声明
public static class User32
{
    # 声明 PostMessage 函数，用于向指定窗口发送消息
    [SecurityCritical]
    [DllImport(Lib.User32, SetLastError = true, CharSet = CharSet.Unicode)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern nint PostMessage(nint hWnd, int msg, int wParam, int lParam);

    # 声明 GetWindowLong 函数，用于获取窗口的指定信息
    [DllImport(Lib.User32)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern int GetWindowLong(nint hWnd, int nIndex);

    # 声明 SetWindowLong 函数，用于设置窗口的指定信息
    [DllImport(Lib.User32)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern int SetWindowLong(nint hWnd, int nIndex, int dwNewLong);

    # 声明 GetCursorPos 函数，用于获取当前光标位置
    [SecurityCritical]
    [DllImport(Lib.User32, SetLastError = true, CharSet = CharSet.Unicode)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool GetCursorPos(out POINT pt);

    # 声明 GetDC 函数，用于获取设备上下文（DC）
    [SecurityCritical]
    [DllImport(Lib.User32, SetLastError = true, CharSet = CharSet.Unicode)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern nint GetDC(nint hWnd);

    # 声明 ReleaseDC 函数，用于释放设备上下文（DC）
    [SecurityCritical]
    [DllImport(Lib.User32, SetLastError = true, CharSet = CharSet.Unicode)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern int ReleaseDC(nint hWnd, nint hDC);

    # 声明 MonitorFromWindow 函数，用于从窗口句柄获取监视器信息
    [SecurityCritical]
    [DllImport(Lib.User32, SetLastError = false, ExactSpelling = true)]
    [DefaultDllImportSearchPaths(DllImportSearchPath.System32)]
    public static extern nint MonitorFromWindow(nint hwnd, MonitorFlags dwFlags);

    # 声明 DestroyIcon 函数，用于销毁图标句柄
    [DllImport(Lib.User32, SetLastError = true)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool DestroyIcon(nint hIcon);

    # 声明 LoadString 函数，用于从资源中加载字符串
    [DllImport(Lib.User32, SetLastError = true, CharSet = CharSet.Unicode)]
    public static extern int LoadString(nint instanceHandle, int id, StringBuilder buffer, int bufferSize);

    # 声明 ClientToScreen 函数，用于将客户端坐标转换为屏幕坐标
    [DllImport(Lib.User32)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool ClientToScreen(nint hwnd, ref POINT point);

    # 声明 GetWindowRect 函数，用于获取窗口的矩形区域
    [DllImport(Lib.User32)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool GetWindowRect(nint hwnd, ref RECT rect);

    # 声明 IsWindow 函数，用于检查窗口句柄是否有效
    [DllImport(Lib.User32, CharSet = CharSet.Auto)]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool IsWindow([In] nint hWnd);

    # 定义 WindowMessage 枚举，包含窗口消息（声明省略）
    public enum WindowMessage
    }

    # 定义 HitTestValues 枚举，包含窗口击测试值（声明省略）
    public enum HitTestValues
    # 定义用于窗口消息处理的常量，表示不同的热区类型
    {
        HTERROR = -2,  # 错误的热区
        HTTRANSPARENT = -1,  # 透明区域
        HTNOWHERE = 0,  # 无热区
        HTCLIENT = 1,  # 客户区
        HTCAPTION = 2,  # 窗口标题栏
        HTSYSMENU = 3,  # 系统菜单区域
        HTGROWBOX = 4,  # 调整窗口大小的控件（通常是右下角的框）
        HTSIZE = HTGROWBOX,  # 调整窗口大小的控件（与 HTGROWBOX 相同）
        HTMENU = 5,  # 菜单区域
        HTHSCROLL = 6,  # 水平滚动条区域
        HTVSCROLL = 7,  # 垂直滚动条区域
        HTMINBUTTON = 8,  # 最小化按钮
        HTMAXBUTTON = 9,  # 最大化按钮
        HTLEFT = 10,  # 窗口左边区域
        HTRIGHT = 11,  # 窗口右边区域
        HTTOP = 12,  # 窗口顶部区域
        HTTOPLEFT = 13,  # 窗口左上角区域
        HTTOPRIGHT = 14,  # 窗口右上角区域
        HTBOTTOM = 15,  # 窗口底部区域
        HTBOTTOMLEFT = 16,  # 窗口左下角区域
        HTBOTTOMRIGHT = 17,  # 窗口右下角区域
        HTBORDER = 18,  # 窗口边框区域
        HTREDUCE = HTMINBUTTON,  # 缩小按钮（与 HTMINBUTTON 相同）
        HTZOOM = HTMAXBUTTON,  # 放大按钮（与 HTMAXBUTTON 相同）
        HTSIZEFIRST = HTLEFT,  # 窗口调整的第一个区域（左边区域）
        HTSIZELAST = HTBOTTOMRIGHT,  # 窗口调整的最后一个区域（右下角区域）
        HTOBJECT = 19,  # 对象区域
        HTCLOSE = 20,  # 关闭按钮
        HTHELP = 21,  # 帮助按钮
    }

    # 定义与窗口属性相关的常量
    [Flags]
    public enum GWL
    {
        GWL_EXSTYLE = -20,  # 扩展窗口样式
        GWLP_HINSTANCE = -6,  # 窗口实例句柄
        GWLP_HWNDPARENT = -8,  # 父窗口句柄
        GWL_ID = -12,  # 窗口标识符
        GWL_STYLE = -16,  # 窗口样式
        GWL_USERDATA = -21,  # 用户数据
        GWL_WNDPROC = -4,  # 窗口过程
        DWLP_USER = 0x8,  # 对话框窗口的用户数据
        DWLP_MSGRESULT = 0x0,  # 对话框窗口的消息结果
        DWLP_DLGPROC = 0x4,  # 对话框窗口的对话过程
    }

    # 定义与窗口样式相关的常量
    [Flags]
    public enum WS : long
    {
        OVERLAPPED = 0x00000000,  # 普通窗口样式
        POPUP = 0x80000000,  # 弹出窗口样式
        CHILD = 0x40000000,  # 子窗口样式
        MINIMIZE = 0x20000000,  # 窗口最小化
        VISIBLE = 0x10000000,  # 窗口可见
        DISABLED = 0x08000000,  # 窗口禁用
        CLIPSIBLINGS = 0x04000000,  # 剪切兄弟窗口
        CLIPCHILDREN = 0x02000000,  # 剪切子窗口
        MAXIMIZE = 0x01000000,  # 窗口最大化
        BORDER = 0x00800000,  # 窗口有边框
        DLGFRAME = 0x00400000,  # 对话框边框
        VSCROLL = 0x00200000,  # 垂直滚动条
        HSCROLL = 0x00100000,  # 水平滚动条
        SYSMENU = 0x00080000,  # 系统菜单
        THICKFRAME = 0x00040000,  # 可调整大小的窗口边框
        GROUP = 0x00020000,  # 窗口属于一组
        TABSTOP = 0x00010000,  # 窗口能够接受 TAB 键

        MINIMIZEBOX = 0x00020000,  # 窗口最小化按钮
        MAXIMIZEBOX = 0x00010000,  # 窗口最大化按钮

        CAPTION = BORDER | DLGFRAME,  # 窗口标题栏
        TILED = OVERLAPPED,  # 窗口平铺样式
        ICONIC = MINIMIZE,  # 窗口最小化样式
        SIZEBOX = THICKFRAME,  # 调整大小框
        TILEDWINDOW = OVERLAPPEDWINDOW,  # 窗口平铺样式（包括标题栏和边框）

        OVERLAPPEDWINDOW = OVERLAPPED | CAPTION | SYSMENU | THICKFRAME | MINIMIZEBOX | MAXIMIZEBOX,  # 完整的普通窗口样式
        POPUPWINDOW = POPUP | BORDER | SYSMENU,  # 弹出窗口样式（包括边框和系统菜单）
        CHILDWINDOW = CHILD,  # 子窗口样式
    }
// 结束一个代码块或者语句块的标志
}
```