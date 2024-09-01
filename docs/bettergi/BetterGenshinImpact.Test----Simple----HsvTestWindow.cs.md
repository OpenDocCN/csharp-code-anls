# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\HsvTestWindow.cs`

```cs
# 引入 OpenCvSharp 库以处理图像处理
﻿using OpenCvSharp;

# 定义命名空间
namespace BetterGenshinImpact.Test.Simple;

# 内部类用于 HSV 测试窗口的操作
internal class HsvTestWindow
{
    # 设定 HSV 中 H 的最大值为 180（360 / 2）
    private const int MaxValueH = 360 / 2;
    # 设定 HSV 中 S 和 V 的最大值为 255
    private static readonly int MaxValue = 255;

    # 声明并初始化 HSV 参数的最小和最大值
    private const string _windowDetectionName = "Object Detection";

    private static int _lowH = 0, _lowS = 0, _lowV = 0;
    private static int _highH = MaxValueH, _highS = MaxValue, _highV = MaxValue;

    # 处理低 H 阈值滑块的回调函数
    static void on_low_H_thresh_trackbar(int pos, nint userdata)
    {
        # 确保低 H 阈值不超过高 H 阈值的前一位
        _lowH = Math.Min(_highH - 1, _lowH);
        # 更新滑块的显示位置
        Cv2.SetTrackbarPos("Low H", _windowDetectionName, _lowH);
    }

    # 处理高 H 阈值滑块的回调函数
    static void on_high_H_thresh_trackbar(int pos, nint userdata)
    {
        # 确保高 H 阈值至少比低 H 阈值大 1
        _highH = Math.Max(_highH, _lowH + 1);
        # 更新滑块的显示位置
        Cv2.SetTrackbarPos("High H", _windowDetectionName, _highH);
    }

    # 处理低 S 阈值滑块的回调函数
    static void on_low_S_thresh_trackbar(int pos, nint userdata)
    {
        # 确保低 S 阈值不超过高 S 阈值的前一位
        _lowS = Math.Min(_highS - 1, _lowS);
        # 更新滑块的显示位置
        Cv2.SetTrackbarPos("Low S", _windowDetectionName, _lowS);
    }

    # 处理高 S 阈值滑块的回调函数
    static void on_high_S_thresh_trackbar(int pos, nint userdata)
    {
        # 确保高 S 阈值至少比低 S 阈值大 1
        _highS = Math.Max(_highS, _lowS + 1);
        # 更新滑块的显示位置
        Cv2.SetTrackbarPos("High S", _windowDetectionName, _highS);
    }

    # 处理低 V 阈值滑块的回调函数
    static void on_low_V_thresh_trackbar(int pos, nint userdata)
    {
        # 确保低 V 阈值不超过高 V 阈值的前一位
        _lowV = Math.Min(_highV - 1, _lowV);
        # 更新滑块的显示位置
        Cv2.SetTrackbarPos("Low V", _windowDetectionName, _lowV);
    }

    # 处理高 V 阈值滑块的回调函数
    static void on_high_V_thresh_trackbar(int pos, nint userdata)
    {
        # 确保高 V 阈值至少比低 V 阈值大 1
        _highV = Math.Max(_highV, _lowV + 1);
        # 更新滑块的显示位置
        Cv2.SetTrackbarPos("High V", _windowDetectionName, _highV);
    }

    # 运行函数的定义，但未实现
    public void Run()
    {
        // 创建一个窗口用于显示图像
        Cv2.NamedWindow(_windowDetectionName);
        // 调整窗口大小为 900x900 像素
        Cv2.ResizeWindow(_windowDetectionName, 900, 900);
        // 创建用于设置 HSV 低阈值的滑块
        Cv2.CreateTrackbar("Low H", _windowDetectionName, ref _lowH, MaxValueH, on_low_H_thresh_trackbar);
        // 创建用于设置 HSV 高阈值的滑块
        Cv2.CreateTrackbar("High H", _windowDetectionName, ref _highH, MaxValueH, on_high_H_thresh_trackbar);
        // 创建用于设置 HSV 低饱和度阈值的滑块
        Cv2.CreateTrackbar("Low S", _windowDetectionName, ref _lowS, MaxValue, on_low_S_thresh_trackbar);
        // 创建用于设置 HSV 高饱和度阈值的滑块
        Cv2.CreateTrackbar("High S", _windowDetectionName, ref _highS, MaxValue, on_high_S_thresh_trackbar);
        // 创建用于设置 HSV 低亮度阈值的滑块
        Cv2.CreateTrackbar("Low V", _windowDetectionName, ref _lowV, MaxValue, on_low_V_thresh_trackbar);
        // 创建用于设置 HSV 高亮度阈值的滑块
        Cv2.CreateTrackbar("High V", _windowDetectionName, ref _highV, MaxValue, on_high_V_thresh_trackbar);
        // 读取图像文件，并以 BGR 颜色空间加载
        var frame = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\自动战斗\队伍识别\bsz1.png", ImreadModes.Color);
        // 创建一个新的矩阵用于存储转换后的 HSV 图像
        Mat frameHsv = new Mat();
        // 将 BGR 图像转换为 HSV 颜色空间
        Cv2.CvtColor(frame, frameHsv, ColorConversionCodes.BGR2HSV);
        // 创建一个新的矩阵用于存储阈值处理后的图像
        Mat frameThreshold = new Mat();

        while (true)
        {
            // 根据 HSV 阈值范围检测图像中的对象
            Cv2.InRange(frameHsv, new Scalar(_lowH, _lowS, _lowV), new Scalar(_highH, _highS, _highV), frameThreshold);
            // 显示处理后的图像
            // Cv2.ImShow(WindowCaptureName, frame);
            Cv2.ImShow(_windowDetectionName, frameThreshold);

            // 等待 30 毫秒并获取用户输入
            char key = (char)Cv2.WaitKey(30);
            // 如果按下 'q' 键或 'Esc' 键，则退出循环
            if (key == 'q' || key == 27)
            {
                break;
            }
        }
    }
# 结束当前代码块或函数体
}
```