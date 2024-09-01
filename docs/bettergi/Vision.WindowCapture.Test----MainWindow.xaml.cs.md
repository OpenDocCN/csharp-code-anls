# `.\better-genshin-impact\Vision.WindowCapture.Test\MainWindow.xaml.cs`

```cs
﻿using System;  // 引入 System 命名空间，提供基本的系统功能
using System.Collections.Generic;  // 引入 System.Collections.Generic 命名空间，提供泛型集合功能
using System.Linq;  // 引入 System.Linq 命名空间，提供 LINQ 查询功能
using System.Text;  // 引入 System.Text 命名空间，提供字符串和文本处理功能
using System.Threading.Tasks;  // 引入 System.Threading.Tasks 命名空间，提供异步编程功能
using System.Windows;  // 引入 System.Windows 命名空间，提供 WPF 窗口和控件功能
using System.Windows.Controls;  // 引入 System.Windows.Controls 命名空间，提供 WPF 控件功能
using System.Windows.Data;  // 引入 System.Windows.Data 命名空间，提供数据绑定功能
using System.Windows.Documents;  // 引入 System.Windows.Documents 命名空间，提供文档和文本功能
using System.Windows.Input;  // 引入 System.Windows.Input 命名空间，提供输入和命令功能
using System.Windows.Interop;  // 引入 System.Windows.Interop 命名空间，提供 WPF 和 Win32 互操作功能
using System.Windows.Media;  // 引入 System.Windows.Media 命名空间，提供绘图和图形功能
using System.Windows.Media.Imaging;  // 引入 System.Windows.Media.Imaging 命名空间，提供图像处理功能
using System.Windows.Navigation;  // 引入 System.Windows.Navigation 命名空间，提供导航功能
using System.Windows.Shapes;  // 引入 System.Windows.Shapes 命名空间，提供几何图形功能

namespace Vision.WindowCapture.Test  // 定义一个名为 Vision.WindowCapture.Test 的命名空间
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window  // 定义一个名为 MainWindow 的部分类，继承自 Window
    {
        public MainWindow()  // MainWindow 类的构造函数
        {
            InitializeComponent();  // 初始化组件，加载 XAML 文件中定义的 UI 元素
        }

        private void CaptureBtn_OnClick(object sender, RoutedEventArgs e)  // 捕获按钮点击事件的处理函数
        {
            var picker = new PickerWindow();  // 创建一个 PickerWindow 实例
            var hWnd = picker.PickCaptureTarget(new WindowInteropHelper(this).Handle);  // 调用 PickerWindow 的 PickCaptureTarget 方法来选择要捕获的窗口，并获取其句柄
            if (hWnd != IntPtr.Zero)  // 如果选择的窗口句柄有效
            {
                var captureWindow = new CaptureTestWindow();  // 创建一个 CaptureTestWindow 实例
                captureWindow.StartCapture(hWnd, CaptureModeEnum.WindowsGraphicsCapture);  // 调用 CaptureTestWindow 的 StartCapture 方法开始捕获指定窗口的内容
                captureWindow.Show();  // 显示 CaptureTestWindow 窗口
            }
        }
    }
}
```