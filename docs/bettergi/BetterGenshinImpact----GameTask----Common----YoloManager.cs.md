# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\YoloManager.cs`

```cs
# 定义一个 C# 命名空间
﻿namespace BetterGenshinImpact.GameTask.Common;

// [Obsolete]
// 注释掉的类定义：YoloManager 类继承自 Singleton<YoloManager>，实现了 IDisposable 接口
// public class YoloManager : Singleton<YoloManager>, IDisposable
// {
//     /// <summary>
//     /// 角色侧面头像分类器的延迟加载对象
//     /// </summary>
//     public readonly Lazy<YoloV8> AvatarSideIconClassifierLazy = new(() => new YoloV8(Global.Absolute("Assets\\Model\\Common\\avatar_side_classify_sim.onnx")));
//
//     // 属性：获取 AvatarSideIconClassifierLazy 的实际值
//     public YoloV8 AvatarSideIconClassifier => AvatarSideIconClassifierLazy.Value;
//
//     // 实现 IDisposable 接口的方法
//     public void Dispose()
//     {
//         // 如果 AvatarSideIconClassifierLazy 的值已创建，则释放其资源
//         if (AvatarSideIconClassifierLazy.IsValueCreated)
//         {
//             AvatarSideIconClassifierLazy.Value.Dispose();
//         }
//     }
// }
```