# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AllMap\FeatureTransfer.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Helpers;
using OpenCvSharp;
using System.IO;

# 定义命名空间
namespace BetterGenshinImpact.Test.Simple.AllMap;

# 定义一个名为 FeatureTransfer 的公共类
public class FeatureTransfer
{
    # 定义一个静态的字符串字段 rootPath，表示根路径
    private static string rootPath = Global.Absolute(@"User\Map\");

    # 定义一个公共的静态方法 Transfer，用于执行特征转移操作
    public static void Transfer()
    {
        # 调用 LoadKeyPointArrayOld 方法加载关键点数组
        var kp = LoadKeyPointArrayOld();
        # 如果加载的关键点数组不为空，则执行以下操作
        if (kp != null)
        {
            # 创建关键点文件的路径，文件名为 mainMap2048Block_SIFT.kp
            string kpPath = Path.Combine(rootPath, $"mainMap2048Block_SIFT.kp");
            # 创建一个 FileStorage 对象，以写入模式打开文件
            FileStorage fs = new(kpPath, FileStorage.Modes.Write);
            # 将关键点数组写入到文件中，键名为 "kp"
            fs.Write("kp", kp);
            # 释放 FileStorage 对象的资源
            fs.Release();
        }
    }

    # 定义一个公共的静态方法 LoadKeyPointArrayOld，用于加载旧版的关键点数组
    public static KeyPoint[]? LoadKeyPointArrayOld()
    {
        # 创建旧版关键点文件的路径，文件名为 mainMap2048Block_SIFT.kp.old
        string kpPath = Path.Combine(rootPath, "mainMap2048Block_SIFT.kp.old");
        # 如果旧版关键点文件存在，则执行以下操作
        if (File.Exists(kpPath))
        {
            # 读取文件内容并反序列化为 KeyPoint 数组
            return ObjectUtils.Deserialize(File.ReadAllBytes(kpPath)) as KeyPoint[];
        }
        # 如果文件不存在，则返回 null
        return null;
    }
}
```