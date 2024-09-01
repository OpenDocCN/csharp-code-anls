# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\HomePageViewModel.cs`

```cs
# 引入 BetterGenshinImpact 项目的核心配置相关类
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact 项目的监控相关类
using BetterGenshinImpact.Core.Monitor;
# 引入 BetterGenshinImpact 项目的 ONNX 识别相关类
using BetterGenshinImpact.Core.Recognition.ONNX;
# 引入 BetterGenshinImpact 项目的游戏任务相关类
using BetterGenshinImpact.GameTask;
# 引入 BetterGenshinImpact 项目的路径管理类
using BetterGenshinImpact.Genshin.Paths;
# 引入 BetterGenshinImpact 项目的辅助工具类
using BetterGenshinImpact.Helpers;
# 引入 BetterGenshinImpact 项目的服务接口类
using BetterGenshinImpact.Service.Interface;
# 引入 BetterGenshinImpact 项目的视图相关类
using BetterGenshinImpact.View;
# 引入 CommunityToolkit 中的 MVVM 组件模型类
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 CommunityToolkit 中的命令输入处理类
using CommunityToolkit.Mvvm.Input;
# 引入 CommunityToolkit 中的消息处理类
using CommunityToolkit.Mvvm.Messaging;
# 引入 CommunityToolkit 中的消息类
using CommunityToolkit.Mvvm.Messaging.Messages;
# 引入 Fischless 游戏捕捉相关类
using Fischless.GameCapture;
# 引入 Microsoft 扩展日志记录类
using Microsoft.Extensions.Logging;
# 引入系统相关功能类
using System;
using System.Diagnostics;
using System.Linq;
using System.Threading.Tasks;
# 引入 Windows 系统互操作相关类
using System.Windows.Interop;
# 引入 Windows 系统功能类
using Windows.System;
# 引入 Wpf.Ui 控件类
using Wpf.Ui.Controls;

# 定义 BetterGenshinImpact.ViewModel.Pages 命名空间
namespace BetterGenshinImpact.ViewModel.Pages;

# 定义 HomePageViewModel 类，继承 ObservableObject 类，实现 INavigationAware 和 IViewModel 接口
public partial class HomePageViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 定义 ObservableProperty 属性，表示游戏捕捉模式名称
    [ObservableProperty] private string[] _modeNames = GameCaptureFactory.ModeNames();

    # 定义 ObservableProperty 属性，表示选中的捕捉模式
    [ObservableProperty] private string? _selectedMode = CaptureModes.BitBlt.ToString();

    # 定义 ObservableProperty 属性，表示任务调度是否启用
    [ObservableProperty] private bool _taskDispatcherEnabled = false;

    # 定义 ObservableProperty 属性，表示开始按钮是否启用，并通知 StartTriggerCommand 命令的可执行状态发生变化
    [ObservableProperty]
    [NotifyCanExecuteChangedFor(nameof(StartTriggerCommand))]
    private bool _startButtonEnabled = true;

    # 定义 ObservableProperty 属性，表示停止按钮是否启用，并通知 StopTriggerCommand 命令的可执行状态发生变化
    [ObservableProperty]
    [NotifyCanExecuteChangedFor(nameof(StopTriggerCommand))]
    private bool _stopButtonEnabled = true;

    # 定义 AllConfig 类型的属性，用于存储配置
    public AllConfig Config { get; set; }

    # 定义 MaskWindow 类型的私有字段，用于表示遮罩窗口
    private MaskWindow? _maskWindow;
    # 定义 ILogger 类型的私有字段，用于日志记录，并初始化为应用程序的 HomePageViewModel 日志记录器
    private readonly ILogger<HomePageViewModel> _logger = App.GetLogger<HomePageViewModel>();

    # 定义 TaskTriggerDispatcher 类型的私有字段，用于任务触发调度
    private readonly TaskTriggerDispatcher _taskDispatcher;
    # 定义 MouseKeyMonitor 类型的私有字段，用于监控鼠标和键盘
    private readonly MouseKeyMonitor _mouseKeyMonitor = new();

    # 定义 IntPtr 类型的私有字段，用于记录上次使用的原神窗口句柄
    private IntPtr _hWnd;

    # 定义 ObservableProperty 属性，表示推理设备类型
    [ObservableProperty]
    private string[] _inferenceDeviceTypes = BgiSessionOption.InferenceDeviceTypes;

    # 定义构造函数，接受 IConfigService 和 TaskTriggerDispatcher 参数
    public HomePageViewModel(IConfigService configService, TaskTriggerDispatcher taskTriggerDispatcher)
    # 设置任务调度器
    {
        _taskDispatcher = taskTriggerDispatcher;
        # 获取配置信息
        Config = configService.Get();
        # 读取游戏安装路径
        ReadGameInstallPath();

        # 检查操作系统版本是否支持 WindowsGraphicsCapture
        // WindowsGraphicsCapture 只支持 Win10 18362 及以上的版本 (Windows 10 version 1903 or later)
        // https://github.com/babalae/better-genshin-impact/issues/394
        if (!OsVersionHelper.IsWindows10_1903_OrGreater)
        {
            # 从模式名称中移除 WindowsGraphicsCapture
            _modeNames = _modeNames.Where(x => x != CaptureModes.WindowsGraphicsCapture.ToString()).ToArray();
            # 从推理设备类型中移除 GPU_DirectML
            // DirectML 是在 Windows 10 版本 1903 和 Windows SDK 的相应版本中引入的。
            // https://learn.microsoft.com/zh-cn/windows/ai/directml/dml
            _inferenceDeviceTypes = _inferenceDeviceTypes.Where(x => x != "GPU_DirectML").ToArray();
        }

        # 注册属性变化消息的处理程序
        WeakReferenceMessenger.Default.Register<PropertyChangedMessage<object>>(this, (sender, msg) =>
        {
            # 如果属性名称为 "Close"，调用 OnClosed 方法
            if (msg.PropertyName == "Close")
            {
                OnClosed();
            }
            # 如果属性名称为 "SwitchTriggerStatus"，根据调度器是否启用决定调用 OnStopTrigger 或 OnStartTriggerAsync
            else if (msg.PropertyName == "SwitchTriggerStatus")
            {
                if (_taskDispatcherEnabled)
                {
                    OnStopTrigger();
                }
                else
                {
                    _ = OnStartTriggerAsync();
                }
            }
        });

        # 获取命令行参数
        var args = Environment.GetCommandLineArgs();
        # 如果命令行参数长度大于1且包含 "start"，调用 OnStartTriggerAsync 方法
        if (args.Length > 1)
        {
            if (args[1].Contains("start"))
            {
                _ = OnStartTriggerAsync();
            }
        }
    }

    # OnLoaded 方法的命令绑定
    [RelayCommand]
    private void OnLoaded()
    {
        # OnTest 方法目前被注释掉
        // OnTest();
    }

    # 处理窗口关闭的逻辑
    private void OnClosed()
    {
        OnStopTrigger();
        # 等待任务结束并关闭遮罩窗口
        _maskWindow?.Close();
    }

    # 捕获模式下拉框改变时的处理方法
    [RelayCommand]
    private async Task OnCaptureModeDropDownChanged()
    {
        # 如果任务调度器启用，则重启
        if (TaskDispatcherEnabled)
        {
            _logger.LogInformation("► 切换捕获模式至[{Mode}]，截图器自动重启...", Config.CaptureMode);
            OnStopTrigger();
            await OnStartTriggerAsync();
        }
    }

    # 推理设备类型下拉框改变时的处理方法
    [RelayCommand]
    private void OnInferenceDeviceTypeDropDownChanged(string value)
    {
    }

    # 开始捕获测试
    [RelayCommand]
    private void OnStartCaptureTest()
    {
        # 显示选择器窗口，并选择捕获目标
        var picker = new PickerWindow();
        var hWnd = picker.PickCaptureTarget(new WindowInteropHelper(UIDispatcherHelper.MainWindow).Handle);
        # 如果成功获取窗口句柄，创建捕获测试窗口并开始捕获
        if (hWnd != IntPtr.Zero)
        {
            var captureWindow = new CaptureTestWindow();
            captureWindow.StartCapture(hWnd, Config.CaptureMode.ToCaptureMode());
            captureWindow.Show();
        }
    }

    # 手动选择窗口并开始捕获
    [RelayCommand]
    private void OnManualPickWindow()
    {
        # 显示选择器窗口，并选择捕获目标
        var picker = new PickerWindow();
        var hWnd = picker.PickCaptureTarget(new WindowInteropHelper(UIDispatcherHelper.MainWindow).Handle);
        # 如果成功获取窗口句柄，保存句柄并开始捕获
        if (hWnd != IntPtr.Zero)
        {
            _hWnd = hWnd;
            Start(hWnd);
        }
        # 否则显示错误消息
        else
        {
            MessageBox.Error("选择的窗体句柄为空！");
        }
    }

    # 命令绑定未完成
    [RelayCommand]
    private async Task OpenDisplayAdvancedGraphicsSettingsAsync()
    {
        // 打开显示设置中的高级图形设置页面
        // ms-settings:display - 显示设置页面 URI
        // ms-settings:display-advancedgraphics - 高级图形设置页面 URI
        // ms-settings:display-advancedgraphics-default - 默认高级图形设置页面 URI
        await Launcher.LaunchUriAsync(new Uri("ms-settings:display-advancedgraphics"));
    }

    private bool CanStartTrigger() => StartButtonEnabled; // 判断是否可以触发开始操作，根据 StartButtonEnabled 属性

    [RelayCommand(CanExecute = nameof(CanStartTrigger))]
    public async Task OnStartTriggerAsync()
    {
        // 查找原神窗口句柄
        var hWnd = SystemControl.FindGenshinImpactHandle();
        if (hWnd == IntPtr.Zero) // 如果未找到窗口
        {
            // 如果配置允许且安装路径不为空
            if (Config.GenshinStartConfig.LinkedStartEnabled && !string.IsNullOrEmpty(Config.GenshinStartConfig.InstallPath))
            {
                // 从本地启动原神并获取窗口句柄
                hWnd = await SystemControl.StartFromLocalAsync(Config.GenshinStartConfig.InstallPath);
                if (hWnd != IntPtr.Zero) // 如果成功启动并获取到窗口句柄
                {
                    // 标识关联启动原神的时间
                    TaskContext.Instance().LinkedStartGenshinTime = DateTime.Now;
                }
            }

            if (hWnd == IntPtr.Zero) // 如果仍未找到窗口
            {
                // 显示错误消息提示用户启动原神
                MessageBox.Error("未找到原神窗口，请先启动原神！");
                return; // 退出方法
            }
        }

        // 启动任务，传入窗口句柄
        Start(hWnd);
    }

    private void Start(IntPtr hWnd)
    {
        if (!TaskDispatcherEnabled) // 如果任务调度器未启用
        {
            _hWnd = hWnd; // 设置窗口句柄
            // 启动任务调度器
            _taskDispatcher.Start(hWnd, Config.CaptureMode.ToCaptureMode(), Config.TriggerInterval);
            // 移除旧的事件处理程序
            _taskDispatcher.UiTaskStopTickEvent -= OnUiTaskStopTick;
            _taskDispatcher.UiTaskStartTickEvent -= OnUiTaskStartTick;
            // 添加新的事件处理程序
            _taskDispatcher.UiTaskStopTickEvent += OnUiTaskStopTick;
            _taskDispatcher.UiTaskStartTickEvent += OnUiTaskStartTick;
            // 如果遮罩窗口为空，则初始化一个新的遮罩窗口并显示
            _maskWindow ??= new MaskWindow();
            _maskWindow.Show();
            // 订阅鼠标和键盘事件
            _mouseKeyMonitor.Subscribe(hWnd);
            TaskDispatcherEnabled = true; // 设置任务调度器启用标志
        }
    }

    private bool CanStopTrigger() => StopButtonEnabled; // 判断是否可以触发停止操作，根据 StopButtonEnabled 属性

    [RelayCommand(CanExecute = nameof(CanStopTrigger))]
    private void OnStopTrigger()
    {
        // 执行停止操作
        Stop();
    }

    private void Stop()
    {
        if (TaskDispatcherEnabled) // 如果任务调度器启用
        {
            // 停止任务调度器
            _taskDispatcher.Stop();
            // 处理遮罩窗口
            if (_maskWindow != null && _maskWindow.IsExist())
            {
                _maskWindow?.Hide(); // 隐藏遮罩窗口
            }
            else
            {
                _maskWindow?.Close(); // 关闭并清除遮罩窗口
                _maskWindow = null;
            }
            TaskDispatcherEnabled = false; // 设置任务调度器禁用标志
            _mouseKeyMonitor.Unsubscribe(); // 取消订阅鼠标和键盘事件
        }
    }

    private void OnUiTaskStopTick(object? sender, EventArgs e)
    {
        // 调用停止操作
        UIDispatcherHelper.Invoke(Stop);
    }

    private void OnUiTaskStartTick(object? sender, EventArgs e)
    {
        // 调用启动操作，传入窗口句柄
        UIDispatcherHelper.Invoke(() => Start(_hWnd));
    }

    public void OnNavigatedTo()
    {
        // 处理导航到此页面时的操作
    }

    public void OnNavigatedFrom()
    {
        // 处理导航离开此页面时的操作
    }

    [RelayCommand]
    public void OnGoToWikiUrl()
    {
        // 打开维基百科链接（假设有实现）
    }
    // 启动默认浏览器打开指定的 URL
    {
        Process.Start(new ProcessStartInfo("https://bgi.huiyadan.com/doc.html") { UseShellExecute = true });
    }

    [RelayCommand]
    private void OnTest()
    {
        // 下面的代码是注释掉的测试代码
        // 通过 OCR 工具识别指定图像中的文字区域
        // var result = OcrFactory.Paddle.OcrResult(new Mat(@"E:\HuiTask\更好的原神\自动秘境\自动战斗\队伍识别\x2.png", ImreadModes.Grayscale));
        // 遍历 OCR 结果中的区域，并打印每个区域的识别文字
        // foreach (var region in result.Regions)
        // {
        //     Debug.WriteLine($"{region.Text}");
        // }

        // 下面的代码是注释掉的 YOLO 模型预测代码
        // 使用 YOLO 模型对指定图片进行检测
        //try
        //{
        //    YoloV8 predictor = new(Global.Absolute("Assets\\Model\\Fish\\bgi_fish.onnx"));
        //    using var memoryStream = new MemoryStream();
        //    new Bitmap(Global.Absolute("test_yolo.png")).Save(memoryStream, ImageFormat.Bmp);
        //    memoryStream.Seek(0, SeekOrigin.Begin);
        //    var result = predictor.Detect(memoryStream);
        //    MessageBox.Show(JsonSerializer.Serialize(result));
        //}
        //catch (Exception e)
        //{
        //    MessageBox.Show(e.StackTrace);
        //}

        // 下面的代码是注释掉的图像模板匹配代码
        // 读取目标图片和源图片
        // Mat tar = new(@"E:\HuiTask\更好的原神\自动剧情\自动邀约\selected.png", ImreadModes.Grayscale);
        // 创建用于模板匹配的掩模
        // var mask = OpenCvCommonHelper.CreateMask(tar, new Scalar(0, 0, 0));
        // 读取源图片
        // var src = new Mat(@"E:\HuiTask\更好的原神\自动剧情\自动邀约\Clip_20240309_135839.png", ImreadModes.Grayscale);
        // 克隆源图片以便绘制结果
        // var src2 = src.Clone();
        // 使用模板匹配查找结果
        // var res = MatchTemplateHelper.MatchOnePicForOnePic(src, mask);
        // // 将匹配结果绘制到克隆图像上
        // foreach (var t in res)
        // {
        //     Cv2.Rectangle(src2, t, new Scalar(0, 0, 255));
        // }
        //
        // // 保存绘制后的图像
        // Cv2.ImWrite(@"E:\HuiTask\更好的原神\自动剧情\自动邀约\x1.png", src2);
    }

    [RelayCommand]
    public async Task SelectInstallPathAsync()
    {
        await Task.Run(() =>
        {
            // 弹出选择文件夹对话框，并设置过滤器
            var dialog = new Ookii.Dialogs.Wpf.VistaOpenFileDialog
            {
                Filter = "原神|YuanShen.exe|原神国际服|GenshinImpact.exe|所有文件|*.*"
            };
            if (dialog.ShowDialog() == true)
            {
                var path = dialog.FileName;
                if (string.IsNullOrEmpty(path))
                {
                    return;
                }

                // 将选择的路径保存到配置中
                Config.GenshinStartConfig.InstallPath = path;
            }
        });
    }

    private void ReadGameInstallPath()
    {
        // 如果配置中没有安装路径，则尝试从注册表读取
        if (string.IsNullOrEmpty(Config.GenshinStartConfig.InstallPath))
        {
            var path = GameExePath.GetWithoutCloud();
            if (!string.IsNullOrEmpty(path))
            {
                // 将读取到的路径保存到配置中
                Config.GenshinStartConfig.InstallPath = path;
            }
        }
    }
这段代码似乎不完整，可能是片段中的结尾符号。你能提供更多上下文吗？
```