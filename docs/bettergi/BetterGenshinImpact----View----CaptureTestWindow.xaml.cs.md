# `.\better-genshin-impact\BetterGenshinImpact\View\CaptureTestWindow.xaml.cs`

```cs
﻿using BetterGenshinImpact.GameTask; // 引入 BetterGenshinImpact.GameTask 命名空间
using BetterGenshinImpact.Helpers.Extensions; // 引入 BetterGenshinImpact.Helpers.Extensions 命名空间
using Fischless.GameCapture; // 引入 Fischless.GameCapture 命名空间
using System; // 引入 System 命名空间
using System.Collections.Generic; // 引入 System.Collections.Generic 命名空间
using System.Diagnostics; // 引入 System.Diagnostics 命名空间
using System.Windows; // 引入 System.Windows 命名空间
using System.Windows.Media; // 引入 System.Windows.Media 命名空间
using Wpf.Ui.Violeta.Controls; // 引入 Wpf.Ui.Violeta.Controls 命名空间

namespace BetterGenshinImpact.View // 定义命名空间 BetterGenshinImpact.View
{
    /// <summary>
    /// CaptureTestWindow.xaml 的交互逻辑
    /// </summary>
    public partial class CaptureTestWindow : Window // 定义 CaptureTestWindow 类，继承自 Window
    {
        private IGameCapture? _capture; // 声明私有字段 _capture，用于捕捉游戏画面，可能为空

        private long _captureTime; // 声明私有字段 _captureTime，记录截图耗时
        private long _transferTime; // 声明私有字段 _transferTime，记录转换耗时
        private long _captureCount; // 声明私有字段 _captureCount，记录截图次数

        public CaptureTestWindow() // CaptureTestWindow 的构造函数
        {
            _captureTime = 0; // 初始化 _captureTime 为 0
            _transferTime = 0; // 初始化 _transferTime 为 0
            _captureCount = 0; // 初始化 _captureCount 为 0
            InitializeComponent(); // 初始化组件，加载 XAML 设计的控件
            Closed += (sender, args) => // 订阅窗口关闭事件
            {
                CompositionTarget.Rendering -= Loop; // 移除 Loop 方法的渲染事件处理
                _capture?.Stop(); // 停止捕捉（如果 _capture 不为空）

                Debug.WriteLine("平均截图耗时:" + _captureTime * 1.0 / _captureCount); // 输出平均截图耗时
                Debug.WriteLine("平均转换耗时:" + _transferTime * 1.0 / _captureCount); // 输出平均转换耗时
                Debug.WriteLine("平均总耗时:" + (_captureTime + _transferTime) * 1.0 / _captureCount); // 输出平均总耗时
            };
        }

        public void StartCapture(IntPtr hWnd, CaptureModes captureMode) // 启动捕捉的方法
        {
            if (hWnd == IntPtr.Zero) // 检查窗口句柄是否为空
            {
                Toast.Warning("请选择窗口"); // 弹出提示，要求选择窗口
                return; // 退出方法
            }

            _capture = GameCaptureFactory.Create(captureMode); // 使用 GameCaptureFactory 创建捕捉实例
            //_capture.IsClientEnabled = true; // 注释掉的代码，可能是为了启用客户端
            _capture.Start(hWnd, // 开始捕捉，并传入窗口句柄和配置
                new Dictionary<string, object>() // 配置参数字典
                {
                    { "useBitmapCache", TaskContext.Instance().Config.WgcUseBitmapCache } // 设置是否使用位图缓存
                }
            );

            CompositionTarget.Rendering += Loop; // 订阅渲染事件，添加 Loop 方法作为处理程序
        }

        private void Loop(object? sender, EventArgs e) // 渲染循环处理方法
        {
            var sw = new Stopwatch(); // 创建 Stopwatch 实例
            sw.Start(); // 开始计时
            var bitmap = _capture?.Capture(); // 捕捉当前画面（如果 _capture 不为空）
            sw.Stop(); // 停止计时
            Debug.WriteLine("截图耗时:" + sw.ElapsedMilliseconds); // 输出截图耗时
            _captureTime += sw.ElapsedMilliseconds; // 累加截图耗时

            if (bitmap != null) // 检查捕捉的位图是否为空
            {
                Debug.WriteLine($"Bitmap:{bitmap.Width}x{bitmap.Height}"); // 输出位图的宽度和高度
                _captureCount++; // 增加截图次数计数
                sw.Reset(); // 重置 Stopwatch 实例
                sw.Start(); // 开始计时
                DisplayCaptureResultImage.Source = bitmap.ToBitmapImage(); // 将捕捉到的位图转换为 BitmapImage 并设置为显示控件的源
                sw.Stop(); // 停止计时
                Debug.WriteLine("转换耗时:" + sw.ElapsedMilliseconds); // 输出转换耗时
                _transferTime += sw.ElapsedMilliseconds; // 累加转换耗时
            }
            else
            {
                Debug.WriteLine("截图失败"); // 输出截图失败信息
            }
        }
    }
}
```