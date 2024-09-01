# `.\better-genshin-impact\Vision.WindowCapture.Test\PickerWindow.xaml.cs`

```cs
# 引入 System 命名空间，包含基本的系统功能
﻿using System;
# 引入 System.ComponentModel 命名空间，包含组件和控件的功能
using System.ComponentModel;
# 引入 System.Diagnostics 命名空间，提供调试相关功能
using System.Diagnostics;
# 引入 System.Linq 命名空间，提供 LINQ 查询功能
using System.Linq;
# 引入 System.Reflection.Metadata 命名空间，提供元数据相关功能
using System.Reflection.Metadata;
# 引入 System.Runtime.InteropServices 命名空间，提供与 COM 互操作的功能
using System.Runtime.InteropServices;
# 引入 System.Text 命名空间，提供字符串处理功能
using System.Text;
# 引入 System.Windows 命名空间，提供 WPF 应用程序的核心功能
using System.Windows;
# 引入 System.Windows.Input 命名空间，提供输入相关功能
using System.Windows.Input;
# 引入 System.Windows.Interop 命名空间，提供 WPF 和 Windows API 互操作功能
using System.Windows.Interop;
# 引入 Windows.Win32 命名空间，提供对 Windows API 的访问
using Windows.Win32;
# 引入 Windows.Win32.Foundation 命名空间，提供 Windows 基础 API 功能
using Windows.Win32.Foundation;
# 使用 Windows.Win32.PInvoke 中的静态方法
using static Windows.Win32.PInvoke;

# 定义命名空间 Vision.WindowCapture.Test
namespace Vision.WindowCapture.Test
{
    # <summary> 注释：PickerWindow.xaml 的交互逻辑 </summary>
    # 定义 PickerWindow 类，继承自 Window
    public partial class PickerWindow : Window
    # 声明一个只读字符串数组，包含要忽略的进程名称
    private readonly string[] _ignoreProcesses = { "applicationframehost", "shellexperiencehost", "systemsettings", "winstore.app", "searchui" };

    # 构造函数，初始化组件并将 OnLoaded 方法注册到 Loaded 事件
    public PickerWindow()
    {
        InitializeComponent();  # 初始化窗口组件
        Loaded += OnLoaded;  # 注册 Loaded 事件处理器
    }

    # Loaded 事件的处理函数
    private void OnLoaded(object sender, RoutedEventArgs e)
    {
        FindWindows();  # 调用 FindWindows 方法以查找窗口
    }

    # 选择捕获目标窗口的方法
    public IntPtr PickCaptureTarget(IntPtr hWnd)
    {
        new WindowInteropHelper(this).Owner = hWnd;  # 设置当前窗口的拥有者为指定的窗口
        ShowDialog();  # 显示对话框

        # 返回选中的窗口的句柄，如果没有选中则返回 IntPtr.Zero
        return ((CapturableWindow?)WindowList.SelectedItem)?.Handle ?? IntPtr.Zero;
    }

    # 查找所有窗口的方法
    private unsafe void FindWindows()
    {
        var wih = new WindowInteropHelper(this);  # 创建当前窗口的 WindowInteropHelper 实例
        EnumWindows((hWnd, lParam) =>
        {
            # 忽略不可见的窗口
            if (!IsWindowVisible(hWnd))
                return true;

            # 忽略没有标题的窗口
            string title;
            int bufferSize = GetWindowTextLength(hWnd) + 1;  # 获取窗口标题的长度，并分配缓冲区
            fixed (char* windowNameChars = new char[bufferSize])
            {
                if (GetWindowText(hWnd, windowNameChars, bufferSize) == 0)  # 获取窗口标题
                {
                    int errorCode = Marshal.GetLastWin32Error();  # 获取错误代码
                    if (errorCode != 0)
                    {
                        throw new Win32Exception(errorCode);  # 抛出 Win32 异常
                    }

                    return true;
                }

                title = new string(windowNameChars);  # 创建标题字符串
                if (string.IsNullOrWhiteSpace(title))  # 如果标题为空或只包含空白字符
                    return true;
            }

            # 忽略当前窗口
            if (wih.Handle == hWnd)
                return true;

            uint processId;
            GetWindowThreadProcessId(hWnd, &processId);  # 获取窗口所属的进程 ID

            # 忽略指定名称的进程
            var process = Process.GetProcessById((int)processId);  # 获取进程对象
            if (_ignoreProcesses.Contains(process.ProcessName.ToLower()))  # 检查进程名称是否在忽略列表中
                return true;

            # 将符合条件的窗口添加到窗口列表
            WindowList.Items.Add(new CapturableWindow
            {
                Handle = hWnd,  # 设置窗口句柄
                Name = $"{title} ({process.ProcessName}.exe)"  # 设置窗口名称
            });

            return true;
        }, IntPtr.Zero);  # 枚举所有窗口
    }

    # 处理鼠标双击事件的方法
    private void WindowsOnMouseDoubleClick(object sender, MouseButtonEventArgs e)
    {
        Close();  # 关闭窗口
    }
}

# 定义一个结构体用于表示可捕获的窗口
public struct CapturableWindow
{
    public string Name { get; set; }  # 窗口名称
    public IntPtr Handle { get; set; }  # 窗口句柄
}
请提供要注释的代码。
```