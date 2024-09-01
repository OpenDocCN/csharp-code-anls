# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AllMap\MatchTemplateTest.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv;
using OpenCvSharp;

namespace BetterGenshinImpact.Test.Simple.AllMap;

public class MatchTemplateTest
{
    // 定义模板大小为 240x135 像素
    public static readonly Size TemplateSize = new(240, 135);

    // 定义裁剪区域（左160，上80，下96）
    public static readonly Rect TemplateSizeRoi = new Rect(20, 10, TemplateSize.Width - 20, TemplateSize.Height - 22);

    public static void Test()
    {
        // 从指定路径加载灰度图像
        var tar = new Mat(@"E:\HuiTask\更好的原神\地图匹配\比较\叠图\无法匹配.png", ImreadModes.Grayscale);
        // 将图像调整为 240x135 像素，并使用立方体插值
        var sTar = tar.Resize(new Size(240, 135), 0, 0, InterpolationFlags.Cubic);
        // 使用注释掉的代码行将调整后的图像裁剪为指定区域
        // sTar = new Mat(sTar, TemplateSizeRoi);
        // 显示调整后的图像
        Cv2.ImShow("sTar", sTar);
        // 从指定路径加载源灰度图像
        var src = new Mat(@"E:\HuiTask\更好的原神\地图匹配\combined_image_small.png", ImreadModes.Grayscale);
        // 克隆源图像以便后续处理
        var src2 = src.Clone();
        // 在源图像中应用模板匹配并获取匹配位置
        var p = MatchTemplateWithGaussianBlur(src, sTar, TemplateMatchModes.CCoeffNormed, null, 0.1);
        // 在克隆的源图像上绘制矩形框标记匹配位置
        Cv2.Rectangle(src2, new Rect(p.X, p.Y, sTar.Width, sTar.Height), new Scalar(0, 0, 255));
        // 将带有矩形框的图像保存到指定路径
        Cv2.ImWrite(@"E:\HuiTask\更好的原神\地图匹配\x1.png", src2);
    }

    public static void TestTrack()
    {
        // 从指定路径加载灰度图像作为目标
        var tar = new Mat(@"E:\HuiTask\更好的原神\自动剧情\任务剧情与地图追踪\blue_task_point_28x.png", ImreadModes.Grayscale);
        // 从指定路径加载灰度图像作为源
        var src = new Mat(@"E:\HuiTask\更好的原神\自动剧情\任务剧情与地图追踪\202404050232291883.png", ImreadModes.Grayscale);
        // 克隆源图像以便后续处理
        var src2 = src.Clone();
        // 在源图像中应用模板匹配并获取匹配位置
        var p = MatchTemplateHelper.MatchTemplate(src, tar, TemplateMatchModes.CCoeffNormed, null, 0.2);
        // 在克隆的源图像上绘制矩形框标记匹配位置
        Cv2.Rectangle(src2, new Rect(p.X, p.Y, tar.Width, tar.Height), new Scalar(0, 0, 255), 1);
        // 将带有矩形框的图像保存到指定路径
        Cv2.ImWrite(@"E:\HuiTask\更好的原神\自动剧情\任务剧情与地图追踪\rec_b1.png", src2);
    }

    public static void TestMenu()
    {
        // 从指定路径加载彩色图像作为目标
        var tar = new Mat(@"D:\HuiPrograming\Projects\CSharp\MiHoYo\BetterGenshinImpact\BetterGenshinImpact\GameTask\Common\Element\Assets\1920x1080\paimon_menu.png", ImreadModes.Color);
        // 从指定路径加载彩色图像作为源
        var src = new Mat(@"E:\HuiTask\更好的原神\地图匹配\比较\菜单\Clip_20240331_155210.png", ImreadModes.Color);
        // 克隆源图像以便后续处理
        var src2 = src.Clone();
        // 在源图像中应用模板匹配并获取匹配位置
        var p = MatchTemplateHelper.MatchTemplate(src, tar, TemplateMatchModes.CCoeffNormed, null, 0.1);
        // 在克隆的源图像上绘制矩形框标记匹配位置
        Cv2.Rectangle(src2, new Rect(p.X, p.Y, tar.Width, tar.Height), new Scalar(0, 0, 255), 1);
        // 绘制小地图的位置矩形框
        // 小地图尺寸 210x210，左上角位置偏移 24,-15
        Cv2.Rectangle(src2, new Rect(p.X + 24, p.Y - 15, 210, 210), Scalar.Blue, 1);
        // 将带有矩形框的图像保存到指定路径
        Cv2.ImWrite(@"E:\HuiTask\更好的原神\地图匹配\比较\菜单\rec2.png", src2);
    }

    public static void TestTeamAvatar()
    {
        # 读取指定路径的图片文件，并以不改变图像深度的模式载入到 Mat 对象中
        var tar = new Mat(@"E:\HuiTask\更好的原神\自动秘境\自动战斗\队伍识别\识别头像素材\UI_AvatarIcon_Side_Zhongli_2.png", ImreadModes.Unchanged);
        # tar = tar.Resize(new Size(108, 108), 0, 0, InterpolationFlags.Cubic); # 被注释的代码：缩放图像到 108x108 大小，使用立方体插值法
        # 分离图像的各个颜色通道
        var channels = tar.Split();
        # 遍历前三个通道（假设图像是 RGBA，i=0,1,2）
        for (int i = 0; i < 3; i++)
        {
            # 将前 3 个通道与第 4 个通道（Alpha 通道）进行按位与运算，保留 Alpha 通道的信息
            channels[i] &= channels[3];
        }
        # 将处理后的通道重新合并为一个图像
        Cv2.Merge(channels[..3], tar);
        # 显示处理后的图像
        Cv2.ImShow("tar.png", tar);
        # 读取另一张图片文件，并以彩色模式载入到 Mat 对象中
        var src = new Mat(@"E:\HuiTask\更好的原神\自动秘境\自动战斗\队伍识别\1.png", ImreadModes.Color);
        # 克隆 src 图像，以便后续使用
        var src2 = src.Clone();
        # 使用模板匹配和高斯模糊来匹配 tar 图像到 src 图像，返回匹配结果的位置
        var p = MatchTemplateWithGaussianBlur(src, tar, TemplateMatchModes.CCoeffNormed, null, 0.1);
        # 在 src2 图像上绘制矩形框，表示匹配的位置
        Cv2.Rectangle(src2, new Rect(p.X, p.Y, tar.Width, tar.Height), new Scalar(0, 0, 255), 1);
        # Cv2.ImWrite(@"E:\HuiTask\更好的原神\自动秘境\自动战斗\队伍识别\rec_1.png", src2); # 被注释的代码：保存 src2 图像到指定路径
        # 显示带有匹配矩形框的图像
        Cv2.ImShow("src2", src2);
    }

    # 静态方法：在 srcMat 图像中使用模板匹配找到 dstMat 图像的位置，支持高斯模糊
    public static Point MatchTemplateWithGaussianBlur(Mat srcMat, Mat dstMat, TemplateMatchModes matchMode, Mat? maskMat = null, double threshold = 0.8)
    {
        try
        {
            # 创建一个空的 Mat 对象来存储匹配结果
            var result = new Mat();
            # 如果没有提供掩模，直接进行模板匹配；否则使用掩模进行匹配
            if (maskMat == null)
            {
                Cv2.MatchTemplate(srcMat, dstMat, result, matchMode);
            }
            else
            {
                Cv2.MatchTemplate(srcMat, dstMat, result, matchMode, maskMat);
            }

            # 根据匹配模式对结果进行归一化处理
            if (matchMode is TemplateMatchModes.SqDiff or TemplateMatchModes.CCoeff or TemplateMatchModes.CCorr)
            {
                Cv2.Normalize(result, result, 0, 1, NormTypes.MinMax, -1, null);
            }
            # 创建一个用于存储模糊处理结果的 Mat 对象
            using var blurResult = new Mat();
            # 对匹配结果进行高斯模糊处理
            Cv2.GaussianBlur(result, blurResult, new Size(1, 1), 0);
            # 从匹配结果中减去模糊结果，增强特征
            Cv2.Subtract(result, blurResult, result);

            # 查找处理后的结果中的最小值和最大值的位置
            Cv2.MinMaxLoc(result, out var minValue, out var maxValue, out var minLoc, out var maxLoc);
            # 显示处理后的匹配结果
            Cv2.ImShow("result", result);
            # 根据匹配模式和阈值确定返回的匹配位置
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

            # 如果没有找到符合条件的匹配，返回默认的 Point (0, 0)
            return new Point();
        }
        catch (Exception ex)
        {
            # 如果发生异常，返回默认的 Point (0, 0)
            return new Point();
        }
    }
# 结束当前代码块或语句
}
```