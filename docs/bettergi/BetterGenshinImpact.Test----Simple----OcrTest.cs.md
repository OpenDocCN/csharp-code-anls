# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\OcrTest.cs`

```cs
﻿using System.Diagnostics; // 引入调试功能
using BetterGenshinImpact.Core.Recognition.OCR; // 引入 OCR 相关功能
using BetterGenshinImpact.Core.Recognition.ONNX.SVTR; // 引入 ONNX SVTR 相关功能
using BetterGenshinImpact.Core.Recognition.OpenCv; // 引入 OpenCV 相关功能
using OpenCvSharp; // 引入 OpenCVSharp 库

namespace BetterGenshinImpact.Test.Simple; // 定义命名空间 BetterGenshinImpact.Test.Simple

public class OcrTest // 定义 OcrTest 类
{
    public static void TestYap() // 定义静态方法 TestYap
    {
        // 读取指定路径的图片文件，并以灰度模式加载到 Mat 对象中
        Mat mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\临时文件\fuben_jueyuan.png", ImreadModes.Grayscale);
        // 使用 TextInferenceFactory 的 Pick 实例进行推断，处理预处理后的 Mat 对象，并返回文本
        var text = TextInferenceFactory.Pick.Inference(PreProcessForInference(mat));
        // 输出推断得到的文本到调试窗口
        Debug.WriteLine(text);

        // 重新读取指定路径的图片文件，并以灰度模式加载到 Mat 对象中
        Mat mat2 = Cv2.ImRead(@"E:\HuiTask\更好的原神\临时文件\fuben_jueyuan.png", ImreadModes.Grayscale);
        // 使用 OcrFactory 的 Paddle 实例进行 OCR 处理，并返回文本
        var text2 = OcrFactory.Paddle.Ocr(mat2);
        // 输出 OCR 处理得到的文本到调试窗口
        Debug.WriteLine(text2);
    }

    private static Mat PreProcessForInference(Mat mat) // 定义静态方法 PreProcessForInference，用于预处理图像
    {
        // Yap 已经改用灰度图了 https://github.com/Alex-Beng/Yap/commit/c2ad1e7b1442aaf2d80782a032e00876cd1c6c84
        // 二值化处理已被注释掉
        // Cv2.Threshold(mat, mat, 0, 255, ThresholdTypes.Otsu | ThresholdTypes.Binary);
        // Cv2.AdaptiveThreshold(mat, mat, 255, AdaptiveThresholdTypes.GaussianC, ThresholdTypes.Binary, 31, 3); // 注释掉的自适应阈值处理方法
        // mat = OpenCvCommonHelper.Threshold(mat, Scalar.FromRgb(235, 235, 235), Scalar.FromRgb(255, 255, 255)); // 注释掉的阈值处理方法
        // 不知道为什么要强制拉伸到 221x32
        mat = ResizeHelper.ResizeTo(mat, 221, 32); // 将图像大小调整为 221x32
        // 填充图像到 384x32 大小
        var padded = new Mat(new Size(384, 32), MatType.CV_8UC1, Scalar.Black); // 创建一个 384x32 的黑色背景图像
        padded[new Rect(0, 0, mat.Width, mat.Height)] = mat; // 将调整后的图像放置到背景图像的左上角
        // Cv2.ImWrite(Global.Absolute("padded.png"), padded); // 注释掉的保存填充图像的操作
        return padded; // 返回填充后的图像
    }
}
```