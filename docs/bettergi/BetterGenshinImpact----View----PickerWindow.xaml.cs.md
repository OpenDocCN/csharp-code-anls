# `.\better-genshin-impact\BetterGenshinImpact\View\PickerWindow.xaml.cs`

```cs
﻿using BetterGenshinImpact.Helpers.DpiAwareness; // 引入 DpiAwareness 帮助类
using System; // 引入基本系统功能
using System.Diagnostics; // 引入进程相关功能
using System.Linq; // 引入 LINQ 扩展方法
using System.Text; // 引入字符串操作功能
using System.Windows; // 引入 WPF 窗口功能
using System.Windows.Input; // 引入输入事件功能
using System.Windows.Interop; // 引入窗口互操作功能
using Vanara.PInvoke; // 引入 Windows API 调用功能

namespace BetterGenshinImpact.View
{
    /// <summary>
    /// PickerWindow.xaml 的交互逻辑
    /// </summary>
    public partial class PickerWindow : Window
    {
        // 要忽略的进程名称列表
        private static readonly string[] _ignoreProcesses = ["applicationframehost", "shellexperiencehost", "systemsettings", "winstore.app", "searchui"];

        public PickerWindow()
        {
            // 初始化组件
            InitializeComponent();
            // 初始化 DPI 感知
            this.InitializeDpiAwareness();
            // 注册窗口加载事件处理程序
            Loaded += OnLoaded;
        }

        private void OnLoaded(object sender, RoutedEventArgs e)
        {
            // 调用查找窗口方法
            FindWindows();
        }

        public IntPtr PickCaptureTarget(IntPtr hWnd)
        {
            // 设置窗口所有者
            new WindowInteropHelper(this).Owner = hWnd;
            // 显示对话框
            ShowDialog();

            // 返回选中的窗口句柄，如果没有选择则返回零
            return ((CapturableWindow?)WindowList.SelectedItem)?.Handle ?? IntPtr.Zero;
        }

        private unsafe void FindWindows()
        {
            // 创建窗口互操作助手
            var wih = new WindowInteropHelper(this);
            // 枚举所有顶级窗口
            User32.EnumWindows((hWnd, lParam) =>
            {
                // 忽略不可见窗口
                if (!User32.IsWindowVisible(hWnd))
                    return true;

                // 忽略没有标题的窗口
                var title = new StringBuilder(1024);
                _ = User32.GetWindowText(hWnd, title, title.Capacity);
                if (string.IsNullOrWhiteSpace(title.ToString()))
                    return true;

                // 忽略当前窗口
                if (wih.Handle == hWnd)
                    return true;

                // 获取窗口所属进程 ID
                _ = User32.GetWindowThreadProcessId(hWnd, out var processId);

                // 忽略指定进程名称的窗口
                var process = Process.GetProcessById((int)processId);
                if (_ignoreProcesses.Contains(process.ProcessName.ToLower()))
                    return true;

                // 将窗口添加到列表
                WindowList.Items.Add(new CapturableWindow
                {
                    Handle = (IntPtr)hWnd,
                    Name = $"{title} ({process.ProcessName}.exe)"
                });

                return true;
            }, IntPtr.Zero);
        }

        private void WindowsOnMouseDoubleClick(object sender, MouseButtonEventArgs e)
        {
            // 关闭窗口
            Close();
        }
    }

    public struct CapturableWindow
    {
        public string Name { get; set; } // 窗口名称
        public IntPtr Handle { get; set; } // 窗口句柄
    }
}
```