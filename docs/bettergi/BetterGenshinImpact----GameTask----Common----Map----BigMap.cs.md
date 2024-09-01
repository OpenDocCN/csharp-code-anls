# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Map\BigMap.cs`

```cs
# 引用 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间下的相关功能
﻿using BetterGenshinImpact.Core.Recognition.OpenCv;
# 引用 BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch 命名空间下的特征匹配功能
using BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch;
# 引用 BetterGenshinImpact.GameTask.Common.Element.Assets 命名空间下的资产管理功能
using BetterGenshinImpact.GameTask.Common.Element.Assets;
# 引用 BetterGenshinImpact.Model 命名空间下的模型功能
using BetterGenshinImpact.Model;
# 引用 OpenCvSharp 命名空间下的 OpenCV 功能
using OpenCvSharp;
# 引用 System.Diagnostics 命名空间下的调试功能
using System.Diagnostics;

# 定义 BetterGenshinImpact.GameTask.Common.Map 命名空间
namespace BetterGenshinImpact.GameTask.Common.Map
{
    /// <summary>
    /// 专门用于大地图的识别
    /// 图像缩小了8倍
    /// </summary>
    # 定义 BigMap 类，继承自 Singleton<BigMap>，实现单例模式
    public class BigMap : Singleton<BigMap>
    {
        # 直接从图像加载特征点
        private readonly FeatureMatcher _featureMatcher = new(MapAssets.Instance.MainMap256BlockMat.Value, new FeatureStorage("mainMap256Block"));

        # 相对标准1024区块的缩放比例
        public const int ScaleTo1024 = 4;

        # 相对2048区块的缩放比例
        public const int ScaleTo2048 = ScaleTo1024 * 2;

        /// <summary>
        /// 基于特征匹配获取地图位置 全部匹配
        /// </summary>
        /// <param name="greyMat">传入的大地图图像会缩小8倍</param>
        /// <returns></returns>
        # 根据特征匹配获取大地图的位置
        public Point2f GetBigMapPositionByFeatureMatch(Mat greyMat)
        {
            try
            {
                # 将图像缩小4倍
                greyMat = ResizeHelper.Resize(greyMat, 1d / 4);

                # 使用特征匹配器匹配缩小后的图像，返回匹配的位置
                return _featureMatcher.Match(greyMat);
            }
            catch
            {
                # 如果匹配失败，输出调试信息并返回默认值
                Debug.WriteLine("Feature Match Failed");
                return new Point2f();
            }
        }

        # 基于特征匹配获取大地图的矩形区域
        public Rect GetBigMapRectByFeatureMatch(Mat greyMat)
        {
            try
            {
                # 将图像缩小4倍
                greyMat = ResizeHelper.Resize(greyMat, 1d / 4);

                # 使用特征匹配器获取缩小后的图像的匹配矩形区域
                return _featureMatcher.KnnMatchRect(greyMat);
            }
            catch
            {
                # 如果匹配失败，输出调试信息并返回空矩形
                Debug.WriteLine("Feature Match Failed");
                return Rect.Empty;
            }
        }
    }
}
```