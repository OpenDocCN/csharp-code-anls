# `.\better-genshin-impact\BetterGenshinImpact\GameTask\QuickTeleport\QuickTeleportTrigger.cs`

```cs
﻿using BetterGenshinImpact.Core.Config; // 引入配置相关的命名空间
using BetterGenshinImpact.Core.Recognition; // 引入识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv; // 引入 OpenCV 识别相关的命名空间
using BetterGenshinImpact.GameTask.Common; // 引入游戏任务相关的通用命名空间
using BetterGenshinImpact.GameTask.Model.Area; // 引入游戏任务中区域模型的命名空间
using BetterGenshinImpact.GameTask.QuickTeleport.Assets; // 引入快速传送任务的资源文件命名空间
using BetterGenshinImpact.Model; // 引入模型相关的命名空间
using Microsoft.Extensions.Logging; // 引入日志记录相关的命名空间
using OpenCvSharp; // 引入 OpenCV Sharp 库
using System; // 引入系统基础类
using System.Linq; // 引入 LINQ 查询功能
using System.Windows.Forms; // 引入 Windows 窗体功能

namespace BetterGenshinImpact.GameTask.QuickTeleport; // 定义命名空间为 BetterGenshinImpact.GameTask.QuickTeleport

internal class QuickTeleportTrigger : ITaskTrigger // 定义内部类 QuickTeleportTrigger，实现 ITaskTrigger 接口
{
    public string Name => "快速传送"; // 属性 Name，返回任务名称 "快速传送"
    public bool IsEnabled { get; set; } // 属性 IsEnabled，表示任务是否启用
    public int Priority => 21; // 属性 Priority，返回任务优先级 21
    public bool IsExclusive { get; set; } // 属性 IsExclusive，表示任务是否独占

    private readonly QuickTeleportAssets _assets; // 私有字段，保存快速传送任务资源实例

    private DateTime _prevClickOptionButtonTime = DateTime.MinValue; // 私有字段，记录上次点击选项按钮的时间

    // private DateTime _prevClickTeleportButtonTime = DateTime.MinValue; // 注释掉的字段，记录上次点击传送按钮的时间
    private DateTime _prevExecute = DateTime.MinValue; // 私有字段，记录上次执行的时间

    private readonly QuickTeleportConfig _config; // 私有字段，保存快速传送配置
    private readonly HotKeyConfig _hotkeyConfig; // 私有字段，保存快捷键配置

    public QuickTeleportTrigger() // 构造函数
    {
        _assets = QuickTeleportAssets.Instance; // 初始化 _assets 为 QuickTeleportAssets 的实例
        _config = TaskContext.Instance().Config.QuickTeleportConfig; // 从任务上下文获取并初始化 _config
        _hotkeyConfig = TaskContext.Instance().Config.HotKeyConfig; // 从任务上下文获取并初始化 _hotkeyConfig
    }

    public void Init() // 初始化方法
    {
        IsEnabled = _config.Enabled; // 设置 IsEnabled 为配置中的启用状态
        IsExclusive = false; // 设置 IsExclusive 为 false
    }

    public void OnCapture(CaptureContent content) // 捕获内容处理方法
    {
        if ((DateTime.Now - _prevExecute).TotalMilliseconds <= 300) // 如果距离上次执行时间小于 300 毫秒
        {
            return; // 直接返回，不进行处理
        }

        // 快捷键传送配置启用的情况下，且快捷键按下的时候激活
        if (_config.HotkeyTpEnabled && !string.IsNullOrEmpty(_hotkeyConfig.QuickTeleportTickHotkey)) // 如果快捷键传送启用且快捷键配置不为空
        {
            if (!IsHotkeyPressed()) // 如果快捷键未被按下
            {
                return; // 直接返回，不进行处理
            }
        }

        _prevExecute = DateTime.Now; // 更新上次执行时间为当前时间

        IsExclusive = false; // 设置 IsExclusive 为 false
        // 1.判断是否在地图界面
        content.CaptureRectArea.Find(_assets.MapScaleButtonRo, _ => // 在捕获内容的区域内查找地图缩放按钮
        {
            IsExclusive = true; // 设置 IsExclusive 为 true

            // 2. 判断是否有传送按钮
            var hasTeleportButton = CheckTeleportButton(content.CaptureRectArea); // 检查捕获区域内是否有传送按钮

            if (!hasTeleportButton) // 如果没有传送按钮
            {
                // 存在地图关闭按钮，说明未选中传送点，直接返回
                var mapCloseRa = content.CaptureRectArea.Find(_assets.MapCloseButtonRo); // 查找地图关闭按钮
                if (!mapCloseRa.IsEmpty()) // 如果地图关闭按钮存在且不为空
                {
                    return; // 直接返回，不进行处理
                }

                // 存在地图选择按钮，说明未选中传送点，直接返回
                var mapChooseRa = content.CaptureRectArea.Find(_assets.MapChooseRo); // 查找地图选择按钮
                if (!mapChooseRa.IsEmpty()) // 如果地图选择按钮存在且不为空
                {
                    return; // 直接返回，不进行处理
                }

                // 3. 循环判断选项列表是否有传送点
                var hasMapChooseIcon = CheckMapChooseIcon(content); // 检查选项列表中是否有传送点图标
                if (hasMapChooseIcon) // 如果存在传送点图标
                {
                    TaskControl.Sleep(_config.WaitTeleportPanelDelay); // 等待传送面板的延迟时间
                    CheckTeleportButton(TaskControl.CaptureToRectArea()); // 重新检查传送按钮
                }
            }
        });
    }
    // 检查是否存在传送按钮，并在找到后点击
    private bool CheckTeleportButton(ImageRegion imageRegion)
    {
        // 初始化变量，用于标记是否找到传送按钮
        var hasTeleportButton = false;
        // 在指定的图像区域内查找传送按钮
        imageRegion.Find(_assets.TeleportButtonRo, ra =>
        {
            // 如果找到按钮，则点击
            ra.Click();
            // 标记已经找到传送按钮
            hasTeleportButton = true;
            // 注释掉的代码：判断点击间隔是否超过1秒，并记录日志
            // if ((DateTime.Now - _prevClickTeleportButtonTime).TotalSeconds > 1)
            // {
            //     TaskControl.Logger.LogInformation("快速传送：传送");
            // }
            // 注释掉的代码：更新上次点击传送按钮的时间
            // _prevClickTeleportButtonTime = DateTime.Now;
        });
        // 返回是否找到传送按钮的结果
        return hasTeleportButton;
    }

    /// <summary>
    /// 全匹配一遍并进行文字识别
    /// 60ms ~200ms
    /// </summary>
    /// <param name="content"></param>
    /// <returns></returns>
    // 声明一个私有方法，用于检查地图选择图标
    private bool CheckMapChooseIcon(CaptureContent content)
    }

    // 注释掉的代码块：用于获取选项的文字
    // /// <summary>
    // /// 获取选项的文字
    // /// </summary>
    // /// <param name="captureMat"></param>
    // /// <param name="foundIconRect"></param>
    // /// <param name="chatOptionTextWidth"></param>
    // /// <returns></returns>
    // [Obsolete]
    // private string GetOptionText(Mat captureMat, Rect foundIconRect, int chatOptionTextWidth)
    // {
    //     // 根据传入的矩形区域定义文字区域
    //     var textRect = new Rect(foundIconRect.X + foundIconRect.Width, foundIconRect.Y, chatOptionTextWidth, foundIconRect.Height);
    //     // 使用捕获的图像和文字区域创建新矩阵
    //     using var mat = new Mat(captureMat, textRect);
    //     // 使用 OCR 工厂对图像进行文字识别
    //     var text = OcrFactory.Paddle.Ocr(mat);
    //     // 返回识别到的文字
    //     return text;
    // }

    // 检查指定的快捷键是否被按下
    private bool IsHotkeyPressed()
    {
        // 检查快捷键是否是鼠标按钮
        if (HotKey.IsMouseButton(_hotkeyConfig.QuickTeleportTickHotkey))
        {
            // 从所有鼠标钩子中查找对应的钩子
            if (MouseHook.AllMouseHooks.TryGetValue((MouseButtons)Enum.Parse(typeof(MouseButtons), _hotkeyConfig.QuickTeleportTickHotkey), out var mouseHook))
            {
                // 如果鼠标按钮被按下，返回 true
                if (mouseHook.IsPressed)
                {
                    return true;
                }
            }
        }
        else
        {
            // 如果快捷键是键盘按钮，从所有键盘钩子中查找对应的钩子
            if (KeyboardHook.AllKeyboardHooks.TryGetValue((Keys)Enum.Parse(typeof(Keys), _hotkeyConfig.QuickTeleportTickHotkey), out var keyboardHook))
            {
                // 如果键盘按钮被按下，返回 true
                if (keyboardHook.IsPressed)
                {
                    return true;
                }
            }
        }

        // 如果没有找到快捷键被按下，返回 false
        return false;
    }
请提供更多代码上下文，这样我才能为你添加注释。
```