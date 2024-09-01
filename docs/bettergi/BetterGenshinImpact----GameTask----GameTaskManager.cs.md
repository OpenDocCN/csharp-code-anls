# `.\better-genshin-impact\BetterGenshinImpact\GameTask\GameTaskManager.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;
# 引入 BetterGenshinImpact.Core.Script.Dependence.Model.TimerConfig 命名空间
using BetterGenshinImpact.Core.Script.Dependence.Model.TimerConfig;
# 引入 BetterGenshinImpact.GameTask.AutoFight.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoFight.Assets;
# 引入 BetterGenshinImpact.GameTask.AutoFishing.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoFishing.Assets;
# 引入 BetterGenshinImpact.GameTask.AutoGeniusInvokation.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Assets;
# 引入 BetterGenshinImpact.GameTask.AutoPick.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoPick.Assets;
# 引入 BetterGenshinImpact.GameTask.AutoSkip.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoSkip.Assets;
# 引入 BetterGenshinImpact.GameTask.AutoWood.Assets 命名空间
using BetterGenshinImpact.GameTask.AutoWood.Assets;
# 引入 BetterGenshinImpact.GameTask.Common.Element.Assets 命名空间
using BetterGenshinImpact.GameTask.Common.Element.Assets;
# 引入 BetterGenshinImpact.GameTask.GameLoading 命名空间
using BetterGenshinImpact.GameTask.GameLoading;
# 引入 BetterGenshinImpact.GameTask.GameLoading.Assets 命名空间
using BetterGenshinImpact.GameTask.GameLoading.Assets;
# 引入 BetterGenshinImpact.GameTask.Placeholder 命名空间
using BetterGenshinImpact.GameTask.Placeholder;
# 引入 BetterGenshinImpact.GameTask.QuickSereniteaPot.Assets 命名空间
using BetterGenshinImpact.GameTask.QuickSereniteaPot.Assets;
# 引入 BetterGenshinImpact.GameTask.QuickTeleport.Assets 命名空间
using BetterGenshinImpact.GameTask.QuickTeleport.Assets;
# 引入 BetterGenshinImpact.View.Drawable 命名空间
using BetterGenshinImpact.View.Drawable;
# 引入 CommunityToolkit.Mvvm.Messaging 命名空间
using CommunityToolkit.Mvvm.Messaging;
# 引入 CommunityToolkit.Mvvm.Messaging.Messages 命名空间
using CommunityToolkit.Mvvm.Messaging.Messages;
# 引入 OpenCvSharp 命名空间
using OpenCvSharp;
# 引入 System.Collections.Concurrent 命名空间
using System.Collections.Concurrent;
# 引入 System.Collections.Generic 命名空间
using System.Collections.Generic;
# 引入 System.IO 命名空间
using System.IO;
# 引入 System.Linq 命名空间
using System.Linq;

# 定义命名空间 BetterGenshinImpact.GameTask
namespace BetterGenshinImpact.GameTask;

# 定义内部类 GameTaskManager
internal class GameTaskManager
{
    # 定义静态属性 TriggerDictionary，类型为 ConcurrentDictionary<string, ITaskTrigger>，可空
    public static ConcurrentDictionary<string, ITaskTrigger>? TriggerDictionary { get; set; }

    /// <summary>
    /// 一定要在任务上下文初始化完毕后使用
    /// </summary>
    /// <returns></returns>
    # 定义静态方法 LoadInitialTriggers，用于加载初始的任务触发器
    public static List<ITaskTrigger> LoadInitialTriggers()
    {
        # 调用 ReloadAssets 方法重新加载资产
        ReloadAssets();
        # 初始化 TriggerDictionary 为新的 ConcurrentDictionary 实例
        TriggerDictionary = new ConcurrentDictionary<string, ITaskTrigger>();

        # 向 TriggerDictionary 中添加不同的触发器实例，使用 TryAdd 方法
        TriggerDictionary.TryAdd("RecognitionTest", new TestTrigger());
        TriggerDictionary.TryAdd("GameLoading", new GameLoadingTrigger());
        TriggerDictionary.TryAdd("AutoPick", new AutoPick.AutoPickTrigger());
        TriggerDictionary.TryAdd("QuickTeleport", new QuickTeleport.QuickTeleportTrigger());
        TriggerDictionary.TryAdd("AutoSkip", new AutoSkip.AutoSkipTrigger());
        TriggerDictionary.TryAdd("AutoFish", new AutoFishing.AutoFishingTrigger());
        TriggerDictionary.TryAdd("AutoCook", new AutoCook.AutoCookTrigger());

        # 调用 ConvertToTriggerList 方法将触发器转换为列表并返回
        return ConvertToTriggerList();
    }

    # 定义静态方法 ConvertToTriggerList，将 TriggerDictionary 转换为触发器列表
    public static List<ITaskTrigger> ConvertToTriggerList(bool allEnabled = false)
    {
        # 如果 TriggerDictionary 为 null，返回空列表
        if (TriggerDictionary is null)
        {
            return [];
        }

        # 将 TriggerDictionary 中的值转换为列表
        var loadedTriggers = TriggerDictionary.Values.ToList();

        # 初始化每个触发器
        loadedTriggers.ForEach(i => i.Init());
        # 如果 allEnabled 为 true，启用所有触发器
        if (allEnabled)
        {
            loadedTriggers.ForEach(i => i.IsEnabled = true);
        }

        # 根据触发器的优先级对列表进行降序排序
        loadedTriggers = [.. loadedTriggers.OrderByDescending(i => i.Priority)];
        # 返回排序后的触发器列表
        return loadedTriggers;
    }

    # 定义静态方法 ClearTriggers，用于清空 TriggerDictionary
    public static void ClearTriggers()
    {
        # 如果 TriggerDictionary 不为 null，调用 Clear 方法清空字典
        TriggerDictionary?.Clear();
    }

    /// <summary>
    /// 通过名称添加触发器
    /// </summary>
    /// <param name="name"></param>
    /// <param name="externalConfig"></param>
    # 定义静态方法 AddTrigger，根据名称添加触发器
    public static void AddTrigger(string name, object? externalConfig)
    {
        // 如果 TriggerDictionary 为 null，则创建新的 ConcurrentDictionary 对象
        TriggerDictionary ??= new ConcurrentDictionary<string, ITaskTrigger>();
        // 清空 TriggerDictionary 中的所有项
        TriggerDictionary.Clear();

        // 如果 name 为 "AutoPick"
        if (name == "AutoPick")
        {
            // 尝试将 AutoPickTrigger 添加到 TriggerDictionary 中
            TriggerDictionary.TryAdd("AutoPick", new AutoPick.AutoPickTrigger(externalConfig as AutoPickExternalConfig));
        }
        // 下面的代码被注释掉了，可能是为了未来的实现
        // else if (name == "AutoSkip")
        // {
        //     TriggerDictionary.Add("AutoSkip", new AutoSkip.AutoSkipTrigger());
        // }
        // else if (name == "AutoFish")
        // {
        //     TriggerDictionary.Add("AutoFish", new AutoFishing.AutoFishingTrigger());
        // }
    }

    // 刷新触发器配置
    public static void RefreshTriggerConfigs()
    {
        // 如果 TriggerDictionary 中有项
        if (TriggerDictionary is { Count: > 0 })
        {
            // 初始化 TriggerDictionary 中指定的触发器
            TriggerDictionary.GetValueOrDefault("AutoPick")?.Init();
            TriggerDictionary.GetValueOrDefault("AutoSkip")?.Init();
            TriggerDictionary.GetValueOrDefault("AutoFish")?.Init();
            TriggerDictionary.GetValueOrDefault("QuickTeleport")?.Init();
            TriggerDictionary.GetValueOrDefault("GameLoading")?.Init();
            TriggerDictionary.GetValueOrDefault("AutoCook")?.Init();
            // 发送消息清理画布
            WeakReferenceMessenger.Default.Send(new PropertyChangedMessage<object>(new object(), "RemoveAllButton", new object(), ""));
            VisionContext.Instance().DrawContent.ClearAll();
        }
        // 重新加载资产
        ReloadAssets();
    }

    // 重新加载所有相关资产
    public static void ReloadAssets()
    {
        AutoPickAssets.DestroyInstance();
        AutoSkipAssets.DestroyInstance();
        AutoFishingAssets.DestroyInstance();
        QuickTeleportAssets.DestroyInstance();
        AutoWoodAssets.DestroyInstance();
        AutoGeniusInvokationAssets.DestroyInstance();
        AutoFightAssets.DestroyInstance();
        ElementAssets.DestroyInstance();
        QuickSereniteaPotAssets.DestroyInstance();
        GameLoadingAssets.DestroyInstance();
    }

    /// <summary>
    /// 获取素材图片并缩放
    /// todo 支持多语言
    /// </summary>
    /// <param name="featName">任务名称</param>
    /// <param name="assertName">素材文件名</param>
    /// <param name="flags">图像读取模式</param>
    /// <returns>返回加载的图像</returns>
    /// <exception cref="FileNotFoundException">如果文件未找到</exception>
    public static Mat LoadAssetImage(string featName, string assertName, ImreadModes flags = ImreadModes.Color)
    {
        // 获取系统信息中的信息
        var info = TaskContext.Instance().SystemInfo;
        // 根据游戏屏幕尺寸和特性名称，生成资产文件夹的绝对路径
        var assetsFolder = Global.Absolute($@"GameTask\{featName}\Assets\{info.GameScreenSize.Width}x{info.GameScreenSize.Height}");
        // 如果指定的资产文件夹不存在，则使用默认的 1920x1080 路径
        if (!Directory.Exists(assetsFolder))
        {
            assetsFolder = Global.Absolute($@"GameTask\{featName}\Assets\1920x1080");
        }

        // 如果资产文件夹仍然不存在，则抛出异常
        if (!Directory.Exists(assetsFolder))
        {
            throw new FileNotFoundException($"未找到{featName}的素材文件夹");
        }

        // 生成文件路径并检查该文件是否存在
        var filePath = Path.Combine(assetsFolder, assertName);
        if (!File.Exists(filePath))
        {
            throw new FileNotFoundException($"未找到{featName}中的{assertName}文件");
        }

        // 从文件流中加载材质
        var mat = Mat.FromStream(File.OpenRead(filePath), flags);
        // 如果屏幕宽度不是 1920，则调整材质大小
        if (info.GameScreenSize.Width != 1920)
        {
            mat = ResizeHelper.Resize(mat, info.AssetScale);
        }

        // 返回材质对象
        return mat;
    }
# 代码结束的大括号，通常用于表示代码块的结束
}
```