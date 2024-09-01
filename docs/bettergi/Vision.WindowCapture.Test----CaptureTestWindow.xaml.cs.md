# `.\better-genshin-impact\Vision.WindowCapture.Test\CaptureTestWindow.xaml.cs`

```cs
﻿using System; // 引入系统基本功能的命名空间
using System.Diagnostics; // 引入用于调试功能的命名空间
using System.Drawing; // 引入 GDI+ 绘图功能的命名空间
using System.IO; // 引入输入输出操作的命名空间
using System.Windows; // 引入 WPF 应用程序的命名空间
using System.Windows.Media; // 引入 WPF 媒体功能的命名空间
using System.Windows.Media.Imaging; // 引入 WPF 图像功能的命名空间
using Windows.Win32.Foundation; // 引入 Windows 原生 API 基础功能的命名空间

namespace Vision.WindowCapture.Test // 定义命名空间
{
    /// <summary>
    /// CaptureTestWindow.xaml 的交互逻辑
    /// </summary>
    public partial class CaptureTestWindow : Window // 定义一个窗口类，继承自 WPF 的 Window 类
    {
        private IWindowCapture? _capture; // 定义一个用于窗口捕获的接口变量

        private long _captureTime; // 定义捕获操作耗时的变量
        private long _transferTime; // 定义转换操作耗时的变量
        private long _captureCount; // 定义捕获操作次数的变量

        public CaptureTestWindow() // 构造函数
        {
            _captureTime = 0; // 初始化捕获时间为 0
            _captureCount = 0; // 初始化捕获次数为 0
            InitializeComponent(); // 初始化组件
            Closed += (sender, args) => // 窗口关闭时的事件处理程序
            {
                CompositionTarget.Rendering -= Loop; // 移除渲染循环中的 Loop 方法
                _capture?.StopAsync(); // 异步停止捕获

                Debug.WriteLine("平均截图耗时:" + _captureTime * 1.0 / _captureCount); // 输出平均截图耗时
                Debug.WriteLine("平均转换耗时:" + _transferTime * 1.0 / _captureCount); // 输出平均转换耗时
                Debug.WriteLine("平均总耗时:" + (_captureTime + _transferTime) * 1.0 / _captureCount); // 输出平均总耗时
            };
        }

        public async void StartCapture(IntPtr hWnd, CaptureModeEnum captureMode) // 启动捕获的方法
        {
            if (hWnd == IntPtr.Zero) // 检查窗口句柄是否有效
            {
                MessageBox.Show("请选择窗口"); // 显示消息框提示用户选择窗口
                return; // 退出方法
            }

            _capture = WindowCaptureFactory.Create(captureMode); // 创建窗口捕获实例
            await _capture.StartAsync((HWND)hWnd); // 异步启动捕获

            CompositionTarget.Rendering += Loop; // 在渲染循环中添加 Loop 方法
        }

        private void Loop(object? sender, EventArgs e) // 渲染循环中的方法
        {
            var sw = new Stopwatch(); // 创建一个计时器
            sw.Start(); // 开始计时
            var bitmap = _capture?.Capture(); // 捕获当前窗口图像
            sw.Stop(); // 停止计时
            Debug.WriteLine("截图耗时:" + sw.ElapsedMilliseconds); // 输出截图耗时
            _captureTime += sw.ElapsedMilliseconds; // 累加截图耗时
            if (bitmap != null) // 检查捕获的图像是否有效
            {
                _captureCount++; // 增加捕获次数
                sw.Reset(); // 重置计时器
                sw.Start(); // 开始计时
                DisplayCaptureResultImage.Source = ToBitmapImage(bitmap); // 将捕获的图像转换为 BitmapImage 并显示
                sw.Stop(); // 停止计时
                Debug.WriteLine("转换耗时:" + sw.ElapsedMilliseconds); // 输出转换耗时
                _transferTime += sw.ElapsedMilliseconds; // 累加转换耗时
            }
            else
            {
                Debug.WriteLine("截图失败"); // 输出截图失败的消息
            }
        }

        public static BitmapImage ToBitmapImage(Bitmap bitmap) // 将 Bitmap 转换为 BitmapImage 的方法
        {
            var ms = new MemoryStream(); // 创建内存流
            bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Bmp); // 将 Bitmap 保存到内存流
            var image = new BitmapImage(); // 创建 BitmapImage 对象
            image.BeginInit(); // 开始初始化 BitmapImage
            ms.Seek(0, SeekOrigin.Begin); // 将内存流的位置设置为开始
            image.StreamSource = ms; // 设置 BitmapImage 的流源为内存流
            image.EndInit(); // 结束初始化 BitmapImage
            return image; // 返回 BitmapImage 对象
        }
    }
}
```