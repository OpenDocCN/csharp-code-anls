# `.\better-genshin-impact\BetterGenshinImpact\View\MaskWindow.xaml.cs`

```cs
﻿using BetterGenshinImpact.Core.Config;  // 引入 BetterGenshinImpact.Core.Config 命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;  // 引入 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间
using BetterGenshinImpact.GameTask;  // 引入 BetterGenshinImpact.GameTask 命名空间
using BetterGenshinImpact.Genshin.Settings;  // 引入 BetterGenshinImpact.Genshin.Settings 命名空间
using BetterGenshinImpact.Helpers;  // 引入 BetterGenshinImpact.Helpers 命名空间
using BetterGenshinImpact.Helpers.DpiAwareness;  // 引入 BetterGenshinImpact.Helpers.DpiAwareness 命名空间
using BetterGenshinImpact.View.Drawable;  // 引入 BetterGenshinImpact.View.Drawable 命名空间
using CommunityToolkit.Mvvm.Messaging;  // 引入 CommunityToolkit.Mvvm.Messaging 命名空间
using CommunityToolkit.Mvvm.Messaging.Messages;  // 引入 CommunityToolkit.Mvvm.Messaging.Messages 命名空间
using Microsoft.Extensions.Logging;  // 引入 Microsoft.Extensions.Logging 命名空间
using Serilog.Sinks.RichTextBox.Abstraction;  // 引入 Serilog.Sinks.RichTextBox.Abstraction 命名空间
using System;  // 引入 System 命名空间
using System.Diagnostics;  // 引入 System.Diagnostics 命名空间
using System.Globalization;  // 引入 System.Globalization 命名空间
using System.Linq;  // 引入 System.Linq 命名空间
using System.Windows;  // 引入 System.Windows 命名空间
using System.Windows.Controls;  // 引入 System.Windows.Controls 命名空间
using System.Windows.Documents;  // 引入 System.Windows.Documents 命名空间
using System.Windows.Interop;  // 引入 System.Windows.Interop 命名空间
using System.Windows.Media;  // 引入 System.Windows.Media 命名空间
using System.Windows.Threading;  // 引入 System.Windows.Threading 命名空间
using Vanara.PInvoke;  // 引入 Vanara.PInvoke 命名空间
using FontFamily = System.Windows.Media.FontFamily;  // 将 System.Windows.Media.FontFamily 映射到 FontFamily 别名

namespace BetterGenshinImpact.View  // 定义 BetterGenshinImpact.View 命名空间
{
    /// <summary>
    /// 一个用于覆盖在游戏窗口上的窗口，用于显示识别结果、显示日志、设置区域位置等
    /// 请使用 Instance 方法获取单例
    /// </summary>
    public partial class MaskWindow : Window  // 定义 MaskWindow 类，继承自 Window
    {
        private static MaskWindow? _maskWindow;  // 定义静态私有字段 _maskWindow，用于存储 MaskWindow 的实例

        private static readonly Typeface _typeface;  // 定义静态只读字段 _typeface，用于存储字体类型

        private nint _hWnd;  // 定义私有字段 _hWnd，用于存储窗口句柄

        private IRichTextBox? _richTextBox;  // 定义私有字段 _richTextBox，用于存储 IRichTextBox 的实例

        private readonly ILogger<MaskWindow> _logger = App.GetLogger<MaskWindow>();  // 定义私有字段 _logger，获取 MaskWindow 的日志记录器

        static MaskWindow()  // 静态构造函数，用于初始化静态成员
        {
            if (Application.Current.TryFindResource("TextThemeFontFamily") is FontFamily fontFamily)  // 尝试从当前应用程序资源中查找字体
            {
                _typeface = fontFamily.GetTypefaces().First();  // 如果找到，则获取第一个 Typeface
            }
            else  // 如果没有找到
            {
                _typeface = new FontFamily("Microsoft Yahei UI").GetTypefaces().First();  // 使用 "Microsoft Yahei UI" 字体，并获取第一个 Typeface
            }

            DefaultStyleKeyProperty.OverrideMetadata(typeof(MaskWindow), new FrameworkPropertyMetadata(typeof(MaskWindow)));  // 设置 MaskWindow 的默认样式键
        }

        public static MaskWindow Instance()  // 定义公共静态方法 Instance，返回 MaskWindow 实例
        {
            if (_maskWindow == null)  // 如果 _maskWindow 为 null
            {
                throw new Exception("MaskWindow 未初始化");  // 抛出异常，表示 MaskWindow 未初始化
            }

            return _maskWindow;  // 返回 _maskWindow 实例
        }

        public bool IsExist()  // 定义公共方法 IsExist，用于检查 MaskWindow 是否存在
        {
            return _maskWindow != null && PresentationSource.FromVisual(_maskWindow) != null;  // 如果 _maskWindow 不为 null 且能够从 _maskWindow 创建 PresentationSource，则返回 true
        }

        public void RefreshPosition()  // 定义公共方法 RefreshPosition，用于刷新窗口位置
        {
            if (TaskContext.Instance().Config.MaskWindowConfig.UseSubform)  // 如果配置中使用了子窗口
            {
                RefreshPositionForSubform();  // 调用 RefreshPositionForSubform 方法刷新位置
            }
            else  // 如果没有使用子窗口
            {
                RefreshPositionForNormal();  // 调用 RefreshPositionForNormal 方法刷新位置
            }
        }

        public void RefreshPositionForNormal()  // 定义公共方法 RefreshPositionForNormal，用于在正常模式下刷新窗口位置
    {
        // 获取当前游戏窗口的捕获矩形区域
        var currentRect = SystemControl.GetCaptureRect(TaskContext.Instance().GameHandle);

        // 在 UI 线程中执行以下代码块
        Invoke(() =>
        {
            // 获取 DPI 缩放因子
            double dpiScale = DpiHelper.ScaleY;

            // 根据 DPI 缩放因子调整矩形的位置和大小
            Left = currentRect.Left / dpiScale;
            Top = currentRect.Top / dpiScale;
            Width = currentRect.Width / dpiScale;
            Height = currentRect.Height / dpiScale;

            // 设置 LogTextBoxWrapper 和 StatusWrapper 控件的位置
            Canvas.SetTop(LogTextBoxWrapper, Height - LogTextBoxWrapper.Height - 65);
            Canvas.SetLeft(LogTextBoxWrapper, 20);
            Canvas.SetTop(StatusWrapper, Height - LogTextBoxWrapper.Height - 90);
            Canvas.SetLeft(StatusWrapper, 20);
        });
        // 发送通知消息以重新计算控件位置
        WeakReferenceMessenger.Default.Send(new PropertyChangedMessage<object>(this, "RefreshSettings", new object(), "重新计算控件位置"));
    }

    public void RefreshPositionForSubform()
    {
        // 获取目标窗口句柄
        nint targetHWnd = TaskContext.Instance().GameHandle;
        // 获取目标窗口的客户区矩形
        _ = User32.GetClientRect(targetHWnd, out RECT targetRect);
        // 获取 DPI 缩放因子
        float x = DpiHelper.GetScale(targetHWnd).X;
        // 设置窗口位置和大小
        _ = User32.SetWindowPos(_hWnd, IntPtr.Zero, 0, 0, (int)(targetRect.Width * x), (int)(targetRect.Height * x), User32.SetWindowPosFlags.SWP_SHOWWINDOW);

        // 在 UI 线程中执行以下代码块
        Invoke(() =>
        {
            // 根据 DPI 缩放因子调整 LogTextBoxWrapper 和 StatusWrapper 控件的位置
            Canvas.SetTop(LogTextBoxWrapper, Height * 1d / DpiHelper.ScaleY - LogTextBoxWrapper.Height * 1d / DpiHelper.ScaleY - 65);
            Canvas.SetLeft(LogTextBoxWrapper, 20);
            Canvas.SetTop(StatusWrapper, Height * 1d / DpiHelper.ScaleY - LogTextBoxWrapper.Height * 1d / DpiHelper.ScaleY - 90);
            Canvas.SetLeft(StatusWrapper, 20);
        });

        // 发送通知消息以重新计算控件位置
        WeakReferenceMessenger.Default.Send(new PropertyChangedMessage<object>(this, "RefreshSettings", new object(), "重新计算控件位置"));
    }

    public MaskWindow()
    {
        // 初始化 _maskWindow 变量为当前实例
        _maskWindow = this;

        // 设置样式资源引用
        this.SetResourceReference(StyleProperty, typeof(MaskWindow));
        // 初始化组件
        InitializeComponent();
        // 初始化 DPI 感知
        this.InitializeDpiAwareness();

        // 注册 LogTextBox 的文本改变事件
        LogTextBox.TextChanged += LogTextBoxTextChanged;
        // 注册 Loaded 事件处理程序
        Loaded += OnLoaded;
    }

    private void OnLoaded(object sender, RoutedEventArgs e)
    {
        // 获取 IRichTextBox 服务
        _richTextBox = App.GetService<IRichTextBox>();
        if (_richTextBox != null)
        {
            // 将 LogTextBox 赋值给 IRichTextBox 的 RichTextBox 属性
            _richTextBox.RichTextBox = LogTextBox;
        }

        // 检查并设置窗口的父窗口
        if (TaskContext.Instance().Config.MaskWindowConfig.UseSubform)
        {
            _hWnd = new WindowInteropHelper(this).Handle;
            nint targetHWnd = TaskContext.Instance().GameHandle;

            if (User32.GetParent(_hWnd) != targetHWnd)
            {
                _ = User32.SetParent(_hWnd, targetHWnd);
            }
        }

        // 刷新控件位置
        RefreshPosition();
        // 打印系统信息
        PrintSystemInfo();
    }

    private void PrintSystemInfo()
    {
        # 记录日志，输出游戏版本信息
        _logger.LogInformation("更好的原神 {Version}", Global.Version);
        # 获取系统信息
        var systemInfo = TaskContext.Instance().SystemInfo;
        # 获取游戏屏幕宽度
        var width = systemInfo.GameScreenSize.Width;
        # 获取游戏屏幕高度
        var height = systemInfo.GameScreenSize.Height;
        # 获取 DPI 缩放值
        var dpiScale = TaskContext.Instance().DpiScale;
        # 记录日志，输出游戏大小、素材缩放和 DPI 缩放信息
        _logger.LogInformation("遮罩窗口已启动，游戏大小{Width}x{Height}，素材缩放{Scale}，DPI缩放{Dpi}",
            width, height, systemInfo.AssetScale.ToString("F"), dpiScale);

        # 检查游戏分辨率是否为 16:9 比例
        if (width * 9 != height * 16)
        {
            # 记录警告日志，指出当前游戏分辨率不是 16:9
            _logger.LogWarning("当前游戏分辨率不是16:9，部分功能可能无法正常使用");
        }

        # 调用方法读取游戏注册表配置
        ReadGameSettings();
    }

    private void ReadGameSettings()
    {
        try
        {
            # 创建设置容器对象
            SettingsContainer settings = new();
            # 设置当前上下文的游戏设置为创建的设置对象
            TaskContext.Instance().GameSettings = settings;
            # 获取游戏语言
            var lang = settings.Language?.TextLang;
            # 检查语言是否为简体中文
            if (lang != null && lang != TextLanguage.SimplifiedChinese)
            {
                # 记录警告日志，指出当前游戏语言不是简体中文
                _logger.LogWarning("当前游戏语言{Lang}不是简体中文，部分功能可能无法正常使用。The game language is not Simplified Chinese, some functions may not work properly", lang);
            }
        }
        catch (Exception e)
        {
            # 记录警告日志，输出游戏注册表配置信息读取失败的详细错误信息
            _logger.LogWarning("游戏注册表配置信息读取失败：" + e.Source + "\r\n--" + Environment.NewLine + e.StackTrace + "\r\n---" + Environment.NewLine + e.Message);
        }
    }

    protected override void OnSourceInitialized(EventArgs e)
    {
        # 调用基类的 OnSourceInitialized 方法
        base.OnSourceInitialized(e);
        # 设置窗口的分层属性
        this.SetLayeredWindow();
        # 设置子窗口属性
        this.SetChildWindow();
        # 隐藏窗口的 Alt+Tab
        this.HideFromAltTab();
    }

    private void LogTextBoxTextChanged(object sender, TextChangedEventArgs e)
    {
        # 获取文本范围
        var textRange = new TextRange(LogTextBox.Document.ContentStart, LogTextBox.Document.ContentEnd);
        # 如果文本长度超过 10000，则清空文本框
        if (textRange.Text.Length > 10000)
        {
            LogTextBox.Document.Blocks.Clear();
        }

        # 将文本框滚动到末尾
        LogTextBox.ScrollToEnd();
    }

    public void Refresh()
    {
        # 调用刷新方法，使界面无效化并重新绘制
        Dispatcher.Invoke(InvalidateVisual);
    }

    public void Invoke(Action action)
    {
        # 执行传入的操作
        Dispatcher.Invoke(action);
    }

    protected override void OnRender(DrawingContext drawingContext)
    {
        try
        {
            // 计算所有矩形、线条和文本的总数量
            var cnt = VisionContext.Instance().DrawContent.RectList.Count + VisionContext.Instance().DrawContent.LineList.Count + VisionContext.Instance().DrawContent.TextList.Count;
            // 如果没有内容，则直接返回
            if (cnt == 0)
            {
                return;
            }

            // 判断是否需要在遮罩上显示识别结果
            if (!TaskContext.Instance().Config.MaskWindowConfig.DisplayRecognitionResultsOnMask)
            {
                return;
            }

            // 遍历矩形列表并绘制非空矩形
            foreach (var kv in VisionContext.Instance().DrawContent.RectList)
            {
                foreach (var drawable in kv.Value)
                {
                    if (!drawable.IsEmpty)
                    {
                        drawingContext.DrawRectangle(Brushes.Transparent,
                            new Pen(new SolidColorBrush(drawable.Pen.Color.ToWindowsColor()), drawable.Pen.Width),
                            drawable.Rect);
                    }
                }
            }

            // 遍历线条列表并绘制所有线条
            foreach (var kv in VisionContext.Instance().DrawContent.LineList)
            {
                foreach (var drawable in kv.Value)
                {
                    drawingContext.DrawLine(new Pen(new SolidColorBrush(drawable.Pen.Color.ToWindowsColor()), drawable.Pen.Width), drawable.P1, drawable.P2);
                }
            }

            // 遍历文本列表并绘制非空文本
            foreach (var kv in VisionContext.Instance().DrawContent.TextList)
            {
                foreach (var drawable in kv.Value)
                {
                    if (!drawable.IsEmpty)
                    {
                        drawingContext.DrawText(new FormattedText(drawable.Text,
                            CultureInfo.GetCultureInfo("zh-cn"),
                            FlowDirection.LeftToRight,
                            _typeface,
                            36, Brushes.Black, 1), drawable.Point);
                    }
                }
            }
        }
        catch (Exception e)
        {
            // 处理异常并输出错误信息到调试输出
            Debug.WriteLine(e);
        }

        // 调用基类的 OnRender 方法
        base.OnRender(drawingContext);
    }

    // 属性用于获取日志框
    public RichTextBox LogBox => LogTextBox;
}
# 定义一个静态类 MaskWindowExtension
file static class MaskWindowExtension
{
    # 将指定的窗口从 Alt+Tab 列表中隐藏
    public static void HideFromAltTab(this Window window)
    {
        # 获取窗口句柄，并调用隐藏函数
        HideFromAltTab(new WindowInteropHelper(window).Handle);
    }

    # 根据窗口句柄将窗口从 Alt+Tab 列表中隐藏
    public static void HideFromAltTab(nint hWnd)
    {
        # 获取窗口的扩展样式
        int style = User32.GetWindowLong(hWnd, User32.WindowLongFlags.GWL_EXSTYLE);

        # 添加 WS_EX_TOOLWINDOW 样式，使窗口从 Alt+Tab 列表中隐藏
        style |= (int)User32.WindowStylesEx.WS_EX_TOOLWINDOW;
        # 设置窗口的新样式
        User32.SetWindowLong(hWnd, User32.WindowLongFlags.GWL_EXSTYLE, style);
    }

    # 将指定的窗口设置为分层窗口，默认启用分层样式
    public static void SetLayeredWindow(this Window window, bool isLayered = true)
    {
        # 获取窗口句柄，并调用设置分层窗口函数
        SetLayeredWindow(new WindowInteropHelper(window).Handle, isLayered);
    }

    # 根据窗口句柄设置窗口是否为分层窗口
    private static void SetLayeredWindow(nint hWnd, bool isLayered = true)
    {
        # 获取窗口的扩展样式
        int style = User32.GetWindowLong(hWnd, User32.WindowLongFlags.GWL_EXSTYLE);

        # 根据是否启用分层样式，调整窗口样式
        if (isLayered)
        {
            # 启用 WS_EX_TRANSPARENT 和 WS_EX_LAYERED 样式
            style |= (int)User32.WindowStylesEx.WS_EX_TRANSPARENT;
            style |= (int)User32.WindowStylesEx.WS_EX_LAYERED;
        }
        else
        {
            # 移除 WS_EX_TRANSPARENT 和 WS_EX_LAYERED 样式
            style &= ~(int)User32.WindowStylesEx.WS_EX_TRANSPARENT;
            style &= ~(int)User32.WindowStylesEx.WS_EX_LAYERED;
        }

        # 设置窗口的新样式
        _ = User32.SetWindowLong(hWnd, User32.WindowLongFlags.GWL_EXSTYLE, style);
    }

    # 将指定的窗口设置为子窗口
    public static void SetChildWindow(this Window window)
    {
        # 获取窗口句柄，并调用设置子窗口函数
        SetChildWindow(new WindowInteropHelper(window).Handle);
    }

    # 根据窗口句柄将窗口设置为子窗口
    private static void SetChildWindow(nint hWnd)
    {
        # 获取窗口的样式
        int style = User32.GetWindowLong(hWnd, User32.WindowLongFlags.GWL_STYLE);

        # 添加 WS_CHILD 样式，将窗口设置为子窗口
        style |= (int)User32.WindowStyles.WS_CHILD;
        # 设置窗口的新样式
        _ = User32.SetWindowLong(hWnd, User32.WindowLongFlags.GWL_STYLE, style);
    }
}
```