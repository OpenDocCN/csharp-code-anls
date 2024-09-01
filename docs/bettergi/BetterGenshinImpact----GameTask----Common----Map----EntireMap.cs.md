# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Map\EntireMap.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch;  // 引用自定义特征匹配库
using BetterGenshinImpact.Helpers.Extensions;  // 引用自定义扩展方法
using BetterGenshinImpact.Model;  // 引用自定义模型
using OpenCvSharp;  // 引用 OpenCV 库
using System;  // 引用基础系统功能
using System.Diagnostics;  // 引用调试功能
using Size = OpenCvSharp.Size;  // 引用 OpenCV 中的 Size 类型，并使用别名 Size

namespace BetterGenshinImpact.GameTask.Common.Map;  // 定义命名空间

// 定义单例类 EntireMap，继承自 Singleton<EntireMap>
public class EntireMap : Singleton<EntireMap>
{
    // 定义模板的大小，宽240，高135
    public static readonly Size TemplateSize = new(240, 135);

    // 定义模板裁剪区域，从(20,10)开始，宽为TemplateSize.Width - 20，高为TemplateSize.Height - 22
    public static readonly Rect TemplateSizeRoi = new(20, 10, TemplateSize.Width - 20, TemplateSize.Height - 22);

    private readonly FeatureMatcher _featureMatcher;  // 声明 FeatureMatcher 实例

    private float _prevX = -1;  // 上一个匹配位置的 X 坐标
    private float _prevY = -1;  // 上一个匹配位置的 Y 坐标

    // 构造函数，初始化 FeatureMatcher 实例
    public EntireMap()
    {
        // 初始化 FeatureMatcher，图像尺寸为28672x26624，特征存储使用“mainMap2048Block”
        _featureMatcher = new FeatureMatcher(new Size(28672, 26624), new FeatureStorage("mainMap2048Block"));
    }

    private int _failCnt = 0;  // 失败计数器

    /// <summary>
    /// 基于特征匹配获取地图位置
    /// 移动匹配
    /// </summary>
    /// <param name="greyMat">灰度图</param>
    /// <param name="mask">遮罩</param>
    /// <returns>匹配位置</returns>
    public Point2f GetMiniMapPositionByFeatureMatch(Mat greyMat, Mat? mask = null)
    {
        try
        {
            Point2f p;
            // 如果上一个位置未设置，则使用默认匹配
            if (_prevX <= 0 && _prevY <= 0)
            {
                p = _featureMatcher.KnnMatch(greyMat, mask);
            }
            else
            {
                // 使用上一个位置和暴力匹配方式进行特征匹配
                p = _featureMatcher.KnnMatch(greyMat, _prevX, _prevY, mask, DescriptorMatcherType.BruteForce);
            }

            // 如果匹配结果为空，则抛出异常
            if (p.IsEmpty())
            {
                throw new InvalidOperationException();
            }
            _prevX = p.X;  // 更新上一个 X 坐标
            _prevY = p.Y;  // 更新上一个 Y 坐标
            _failCnt = 0;  // 重置失败计数器
            return p;  // 返回匹配位置
        }
        catch
        {
            // 如果匹配失败，记录日志并增加失败计数器
            Debug.WriteLine("Feature Match Failed");
            _failCnt++;
            // 如果失败次数超过5次，则重置位置并重新开始匹配
            if (_failCnt > 5)
            {
                Debug.WriteLine("Feature Match Failed Too Many Times, 重新从全地图进行特征匹配");
                _failCnt = 0;  // 重置失败计数器
                (_prevX, _prevY) = (-1, -1);  // 重置上一个位置
            }
            return new Point2f();  // 返回空的匹配位置
        }
    }

    /// <summary>
    /// 基于特征匹配获取地图位置 全部匹配
    /// </summary>
    /// <param name="greyMat">灰度图</param>
    /// <returns>匹配位置</returns>
    public Point2f GetBigMapPositionByFeatureMatch(Mat greyMat)
    {
        try
        {
            // 使用 FeatureMatcher 进行匹配
            var p = _featureMatcher.Match(greyMat);
            // 如果匹配结果为空，则抛出异常
            if (p.IsEmpty())
            {
                throw new InvalidOperationException();
            }
            return p;  // 返回匹配位置
        }
        catch
        {
            // 如果匹配失败，记录日志
            Debug.WriteLine("Feature Match Failed");
            return new Point2f();  // 返回空的匹配位置
        }
    }
}
```