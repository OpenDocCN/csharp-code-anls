# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Model\BaseAssets.cs`

```cs
# 引用 BetterGenshinImpact.Model 和 OpenCvSharp 命名空间
﻿using BetterGenshinImpact.Model;
using OpenCvSharp;

# 定义命名空间 BetterGenshinImpact.GameTask.Model
namespace BetterGenshinImpact.GameTask.Model;

/// <summary>
/// 游戏各类任务的素材基类
/// 必须继承自 BaseAssets
/// 且必须晚于 TaskContext 初始化，也就是 TaskContext.Instance().IsInitialized = true;
/// 在整个任务生命周期开始时, 必须先使用 DestroyInstance() 销毁实例, 保证资源的类型正确引用
/// </summary>
/// <typeparam name="T"></typeparam>
public class BaseAssets<T> : Singleton<T> where T : class
{
    # 获取任务上下文中系统信息的 ScaleMax1080P 捕捉区域的矩形
    protected Rect CaptureRect => TaskContext.Instance().SystemInfo.ScaleMax1080PCaptureRect;
    
    # 获取任务上下文中系统信息的资产缩放比例
    protected double AssetScale => TaskContext.Instance().SystemInfo.AssetScale;

    // 以下为注释掉的旧代码
    // private int _gameWidth;
    // private int _gameHeight;
    //
    // public new static T Instance
    // {
    //     get
    //     {
    //         // 统一在这里处理重新生成实例
    //         if (_instance != null)
    //         {
    //             var r = TaskContext.Instance().SystemInfo.CaptureAreaRect;
    //             if (_instance is BaseAssets<T> baseAssets)
    //             {
    //                 if (baseAssets._gameWidth != r.Width || baseAssets._gameHeight != r.Height)
    //                 {
    //                     baseAssets._gameWidth = r.Width;
    //                     baseAssets._gameHeight = r.Height;
    //                     _instance = null;
    //                 }
    //             }
    //         }
    //         return LazyInitializer.EnsureInitialized(ref _instance, ref syncRoot, CreateInstance);
    //     }
    // }
}
```