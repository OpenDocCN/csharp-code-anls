# `.\better-genshin-impact\BetterGenshinImpact\GameTask\GameLoading\GameLoading.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间
using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.GameTask.GameLoading.Assets 命名空间
using BetterGenshinImpact.GameTask.GameLoading.Assets;
# 引入 System 命名空间
using System;
# 引入 System.Diagnostics 命名空间
using System.Diagnostics;

# 定义命名空间 BetterGenshinImpact.GameTask.GameLoading
namespace BetterGenshinImpact.GameTask.GameLoading;

# 定义 GameLoadingTrigger 类，继承 ITaskTrigger 接口
public class GameLoadingTrigger : ITaskTrigger
{
    # 只读属性 Name，返回 "自动开门"
    public string Name => "自动开门";

    # 属性 IsEnabled，表示该任务是否启用
    public bool IsEnabled { get; set; }

    # 只读属性 Priority，返回任务的优先级
    public int Priority => 999;

    # 只读属性 IsExclusive，表示该任务是否独占资源
    public bool IsExclusive => false;

    # 只读属性 IsBackgroundRunning，表示任务是否在后台运行
    public bool IsBackgroundRunning => true;

    # 私有字段 _assets，类型为 GameLoadingAssets
    private readonly GameLoadingAssets _assets;

    # 私有字段 _config，获取 TaskContext 实例中的 GenshinStartConfig 配置
    private readonly GenshinStartConfig _config = TaskContext.Instance().Config.GenshinStartConfig;

    # 私有字段 _enterGameClickCount，用于记录点击进入游戏的次数
    private int _enterGameClickCount = 0;
    # 私有字段 _welkinMoonClickCount，用于记录点击月亮的次数
    private int _welkinMoonClickCount = 0;
    # 私有字段 _noneClickCount 和 _wmNoneClickCount，用于记录其他点击次数
    private int _noneClickCount, _wmNoneClickCount;

    # 私有字段 _prevExecuteTime，记录上次执行时间，初始化为最小日期时间
    private DateTime _prevExecuteTime = DateTime.MinValue;

    # 构造函数 GameLoadingTrigger
    public GameLoadingTrigger()
    {
        # 销毁 GameLoadingAssets 实例
        GameLoadingAssets.DestroyInstance();
        # 获取 GameLoadingAssets 实例
        _assets = GameLoadingAssets.Instance;
    }

    # 初始化方法 Init
    public void Init()
    {
        # 根据配置设置 IsEnabled 属性
        IsEnabled = _config.AutoEnterGameEnabled;
        # 如果自上次联动启动原神时间已超过 5 分钟，则禁用该任务
        if ((DateTime.Now - TaskContext.Instance().LinkedStartGenshinTime).TotalMinutes >= 5)
        {
            IsEnabled = false;
        }

        # 重置 _enterGameClickCount
        _enterGameClickCount = 0;
    }

    # 方法 OnCapture，接收 CaptureContent 类型的参数 content
    public void OnCapture(CaptureContent content)
    {
        // 如果距离上次执行时间小于或等于 5000 毫秒（5秒），则不执行后续代码
        if ((DateTime.Now - _prevExecuteTime).TotalMilliseconds <= 5000)
        {
            return;
        }
        // 更新上次执行时间为当前时间
        _prevExecuteTime = DateTime.Now;
        // 如果当前时间距离任务开始时间已超过 5 分钟，则禁用功能并返回
        if ((DateTime.Now - TaskContext.Instance().LinkedStartGenshinTime).TotalMinutes >= 5)
        {
            IsEnabled = false;
            return;
        }

        // 查找指定的游戏区域
        using var ra = content.CaptureRectArea.Find(_assets.EnterGameRo);
        if (!ra.IsEmpty())
        {
            // 如果找到指定区域，点击背景并增加进入游戏点击计数
            TaskContext.Instance().PostMessageSimulator.LeftButtonClickBackground();
            _enterGameClickCount++;
        }
        else
        {
            // 如果没有找到指定区域
            if (_enterGameClickCount > 0 && !_config.AutoClickBlessingOfTheWelkinMoonEnabled)
            {
                // 增加未点击计数，如果超过 5 次则禁用功能
                _noneClickCount++;
                if (_noneClickCount > 5)
                {
                    IsEnabled = false;
                }
            }
        }

        // 如果进入游戏点击计数大于 0 并且自动点击祝福功能启用
        if (_enterGameClickCount > 0 && _config.AutoClickBlessingOfTheWelkinMoonEnabled)
        {
            // 查找祝福区域
            var wmRa = content.CaptureRectArea.Find(_assets.WelkinMoonRo);
            if (!wmRa.IsEmpty())
            {
                // 如果找到祝福区域，点击背景，增加祝福点击计数并记录日志
                TaskContext.Instance().PostMessageSimulator.LeftButtonClickBackground();
                _welkinMoonClickCount++;
                Debug.WriteLine("[GameLoading] Click blessing of the welkin moon");
                // 如果祝福点击计数超过 2 次，则禁用功能
                if (_welkinMoonClickCount > 2)
                {
                    IsEnabled = false;
                }
            }
            else
            {
                // 如果没有找到祝福区域
                if (_welkinMoonClickCount > 0)
                {
                    // 增加祝福未点击计数，如果超过 1 次则禁用功能
                    _wmNoneClickCount++;
                    if (_wmNoneClickCount > 1)
                    {
                        IsEnabled = false;
                    }
                }
            }
        }
    }
这个代码块看起来是个不完整的片段，能否提供更多上下文或者完整的代码？这样我可以更好地帮助你添加注释。
```