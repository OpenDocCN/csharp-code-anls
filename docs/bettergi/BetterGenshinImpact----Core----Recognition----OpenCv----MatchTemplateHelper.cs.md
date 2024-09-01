# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\MatchTemplateHelper.cs`

```cs
# 使用 Microsoft.Extensions.Logging 库用于日志记录
﻿using Microsoft.Extensions.Logging;
# 使用 OpenCvSharp 库进行图像处理
using OpenCvSharp;
# 引入基本系统功能
using System;
# 引入泛型集合
using System.Collections.Generic;

# 定义命名空间
namespace BetterGenshinImpact.Core.Recognition.OpenCv
{
    /// <summary>
    ///     多目标模板匹配demo
    ///     https://github.com/shimat/opencvsharp/issues/182
    /// </summary>
    public class MatchTemplateHelper
    {
        # 创建一个静态日志记录器实例
        private static readonly ILogger<MatchTemplateHelper> _logger = App.GetLogger<MatchTemplateHelper>();

        /// <summary>
        ///  模板匹配
        /// </summary>
        /// <param name="srcMat">原图像</param>
        /// <param name="dstMat">模板</param>
        /// <param name="matchMode">匹配方式</param>
        /// <param name="maskMat">遮罩</param>
        /// <param name="threshold">阈值</param>
        /// <returns>左上角的标点,由于(0,0)点作为未匹配的结果，所以不能做完全相同的模板匹配</returns>
        public static Point MatchTemplate(Mat srcMat, Mat dstMat, TemplateMatchModes matchMode, Mat? maskMat = null, double threshold = 0.8)
        {
            try
            {
                # 创建一个新的 Mat 对象用于存储匹配结果
                using var result = new Mat();
                # 执行模板匹配操作
                Cv2.MatchTemplate(srcMat, dstMat, result, matchMode, maskMat!);

                # 根据匹配模式对结果进行归一化处理
                if (matchMode is TemplateMatchModes.SqDiff or TemplateMatchModes.CCoeff or TemplateMatchModes.CCorr)
                {
                    Cv2.Normalize(result, result, 0, 1, NormTypes.MinMax);
                }

                # 查找匹配结果中的最小值和最大值，以及它们的位置
                Cv2.MinMaxLoc(result, out var minValue, out var maxValue, out var minLoc, out var maxLoc);

                # 根据匹配模式判断是否符合阈值条件，并返回对应的位置
                if (matchMode is TemplateMatchModes.SqDiff or TemplateMatchModes.SqDiffNormed)
                {
                    if (minValue <= 1 - threshold)
                    {
                        return minLoc;
                    }
                }
                else
                {
                    if (maxValue >= threshold)
                    {
                        return maxLoc;
                    }
                }

                # 如果没有匹配结果，返回默认值
                return default;
            }
            catch (Exception ex)
            {
                # 记录错误日志
                _logger.LogError(ex.Message);
                # 记录调试日志
                _logger.LogDebug(ex, ex.Message);
                # 返回默认值
                return default;
            }
        }

        /// <summary>
        ///     模板匹配多个结果
        ///     不好用
        /// </summary>
        /// <param name="srcMat"></param>
        /// <param name="dstMat"></param>
        /// <param name="maskMat"></param>
        /// <param name="threshold"></param>
        /// <param name="maxCount"></param>
        /// <returns></returns>
        [Obsolete]
        public static List<Point> MatchTemplateMulti(Mat srcMat, Mat dstMat, Mat? maskMat = null, double threshold = 0.8, int maxCount = 8)
    {
        // 创建一个点的列表，用于存储匹配结果
        var points = new List<Point>();
        try
        {
            // 创建一个 Mat 对象，用于存储匹配结果
            using var result = new Mat();
            // 在 srcMat 中寻找与 dstMat 匹配的模板，结果存储在 result 中，使用掩码 maskMat
            Cv2.MatchTemplate(srcMat, dstMat, result, TemplateMatchModes.CCoeffNormed, maskMat!);

            // 创建一个掩码图像，用于标记匹配区域
            var mask = new Mat(result.Height, result.Width, MatType.CV_8UC1, Scalar.White);
            // 创建一个子掩码图像，用于在匹配区域上绘制矩形
            var maskSub = new Mat(result.Height, result.Width, MatType.CV_8UC1, Scalar.Black);
            // 无限循环，用于找到所有匹配区域
            while (true)
            {
                // 查找 result 中的最大值及其位置，使用掩码 mask
                Cv2.MinMaxLoc(result, out _, out var maxValue, out _, out var maxLoc, mask);
                // 创建一个矩形区域，用于遮盖已经找到的匹配
                var maskRect = new Rect(maxLoc.X, maxLoc.Y, dstMat.Width, dstMat.Height);
                // 在子掩码上绘制矩形区域
                maskSub.Rectangle(maskRect, Scalar.White, -1);
                // 从掩码中减去子掩码区域
                mask -= maskSub;
                // 如果匹配值超过阈值，则将最大位置添加到结果列表中
                if (maxValue >= threshold)
                    points.Add(maxLoc);
                else
                    // 如果匹配值低于阈值，则退出循环
                    break;
            }

            // 返回匹配到的点列表
            return points;
        }
        catch (Exception ex)
        {
            // 记录错误信息
            _logger.LogError(ex.Message);
            // 记录调试信息
            _logger.LogDebug(ex, ex.Message);
            // 返回空的点列表
            return points;
        }
    }

    // 调用 MatchTemplateMulti 方法，使用默认的 maskMat
    public static List<Point> MatchTemplateMulti(Mat srcMat, Mat dstMat, double threshold)
    {
        return MatchTemplateMulti(srcMat, dstMat, null, threshold);
    }

    /// <summary>
    ///     在一张图中查找多个模板
    ///     查到一个遮盖一个的笨方法，效率很低，但是很准确
    /// </summary>
    /// <param name="srcMat">源图像</param>
    /// <param name="imgSubDictionary">模板图像字典</param>
    /// <param name="threshold">匹配阈值</param>
    /// <returns>模板匹配结果的字典</returns>
    public static Dictionary<string, List<Point>> MatchMultiPicForOnePic(Mat srcMat, Dictionary<string, Mat> imgSubDictionary, double threshold = 0.8)
    {
        // 创建一个字典，用于存储每个模板的匹配结果
        var dictionary = new Dictionary<string, List<Point>>();
        // 遍历每个模板
        foreach (var kvp in imgSubDictionary)
        {
            // 创建一个点的列表，用于存储当前模板的匹配结果
            var list = new List<Point>();

            // 无限循环，用于找到当前模板的所有匹配区域
            while (true)
            {
                // 调用 MatchTemplate 方法找到一个匹配点
                var point = MatchTemplate(srcMat, kvp.Value, TemplateMatchModes.CCoeffNormed, null, threshold);
                if (point != new Point())
                {
                    // 在源图像上绘制一个黑色矩形以遮盖匹配区域
                    Cv2.Rectangle(srcMat, point, new Point(point.X + kvp.Value.Width, point.Y + kvp.Value.Height), Scalar.Black, -1);
                    // 将匹配点添加到列表中
                    list.Add(point);
                }
                else
                {
                    // 如果没有找到匹配点，则退出循环
                    break;
                }
            }

            // 将模板名称和匹配点列表添加到字典中
            dictionary.Add(kvp.Key, list);
        }

        // 返回包含所有模板匹配结果的字典
        return dictionary;
    }

    /// <summary>
    ///     在一张图中查找多个模板
    ///     查到一个遮盖一个的笨方法，效率很低，但是很准确
    /// </summary>
    /// <param name="srcMat">源图像</param>
    /// <param name="imgSubList">模板图像列表</param>
    /// <param name="threshold">匹配阈值</param>
    /// <returns>模板匹配结果的矩形列表</returns>
    public static List<Rect> MatchMultiPicForOnePic(Mat srcMat, List<Mat> imgSubList, double threshold = 0.8)
    # 创建一个矩形区域列表
    List<Rect> list = [];
    # 遍历 imgSubList 中的每个子模板
    foreach (var sub in imgSubList)
        # 不断尝试在图像中找到当前子模板
        while (true)
        {
            # 使用模板匹配查找子模板的最佳匹配位置
            var point = MatchTemplate(srcMat, sub, TemplateMatchModes.CCoeffNormed, null, threshold);
            # 如果找到匹配位置
            if (point != new Point())
            {
                # 使用黑色矩形遮掩掉匹配结果，避免重复识别
                Cv2.Rectangle(srcMat, point, new Point(point.X + sub.Width, point.Y + sub.Height), Scalar.Black, -1);
                # 将找到的矩形区域添加到列表中
                list.Add(new Rect(point.X, point.Y, sub.Width, sub.Height));
            }
            else
            {
                # 如果没有找到匹配位置，则跳出循环
                break;
            }
        }

    # 返回找到的所有矩形区域
    return list;



    /// <summary>
    ///     在一张图中查找一个个模板
    /// </summary>
    /// <param name="srcMat"></param>
    /// <param name="dstMat"></param>
    /// <param name="maskMat"></param>
    /// <param name="threshold"></param>
    /// <returns></returns>
    # 定义一个方法来在图像中查找模板，返回找到的矩形区域列表
    public static List<Rect> MatchOnePicForOnePic(Mat srcMat, Mat dstMat, Mat? maskMat = null, double threshold = 0.8)
    {
        # 创建一个矩形区域列表
        List<Rect> list = [];

        # 不断尝试在图像中找到目标模板
        while (true)
        {
            # 使用模板匹配查找目标模板的最佳匹配位置
            var point = MatchTemplate(srcMat, dstMat, TemplateMatchModes.CCoeffNormed, maskMat, threshold);
            # 如果找到匹配位置
            if (point != new Point())
            {
                # 使用黑色矩形遮掩掉匹配结果，避免重复识别
                Cv2.Rectangle(srcMat, point, new Point(point.X + dstMat.Width, point.Y + dstMat.Height), Scalar.Black, -1);
                # 将找到的矩形区域添加到列表中
                list.Add(new Rect(point.X, point.Y, dstMat.Width, dstMat.Height));
            }
            else
            {
                # 如果没有找到匹配位置，则跳出循环
                break;
            }
        }

        # 返回找到的所有矩形区域
        return list;
    }



    /// <summary>
    ///    在一张图中查找一个个模板
    /// </summary>
    /// <param name="srcMat"></param>
    /// <param name="dstMat"></param>
    /// <param name="matchMode"></param>
    /// <param name="maskMat"></param>
    /// <param name="threshold"></param>
    /// <param name="maxCount"></param>
    /// <returns></returns>
    # 定义一个方法来在图像中查找模板，返回找到的矩形区域列表，允许设置最大查找次数
    public static List<Rect> MatchOnePicForOnePic(Mat srcMat, Mat dstMat, TemplateMatchModes matchMode, Mat? maskMat, double threshold, int maxCount = -1)
    {
        # 创建一个矩形区域列表
        List<Rect> list = [];

        # 如果没有指定最大查找次数，计算一个默认值
        if (maxCount < 0)
        {
            maxCount = srcMat.Width * srcMat.Height / dstMat.Width / dstMat.Height;
        }

        # 限制查找次数，避免过多计算
        for (int i = 0; i < maxCount; i++)
        {
            # 使用模板匹配查找目标模板的最佳匹配位置
            var point = MatchTemplate(srcMat, dstMat, matchMode, maskMat, threshold);
            # 如果找到匹配位置
            if (point != new Point())
            {
                # 使用黑色矩形遮掩掉匹配结果，避免重复识别
                Cv2.Rectangle(srcMat, point, new Point(point.X + dstMat.Width, point.Y + dstMat.Height), Scalar.Black, -1);
                # 将找到的矩形区域添加到列表中
                list.Add(new Rect(point.X, point.Y, dstMat.Width, dstMat.Height));
            }
            else
            {
                # 如果没有找到匹配位置，则跳出循环
                break;
            }
        }

        # 返回找到的所有矩形区域
        return list;
    }
# 代码块的结束符
}
```