# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Placeholder\PlaceholderTrigger.cs`

```cs
﻿using OpenCvSharp;  // 引入 OpenCVSharp 库
using System;  // 引入 System 命名空间
using System.Collections.Generic;  // 引入集合类
using System.Drawing;  // 引入绘图功能
using System.Linq;  // 引入 LINQ 查询功能
using Point = OpenCvSharp.Point;  // 定义 Point 别名为 OpenCvSharp.Point
using Size = OpenCvSharp.Size;  // 定义 Size 别名为 OpenCvSharp.Size

namespace BetterGenshinImpact.GameTask.Placeholder
{
    /// <summary>
    /// 一个用于开发测试的识别、或者全局占位触发器
    /// 这个触发器启动的时候，直接独占
    /// </summary>
    public class TestTrigger : ITaskTrigger
    {
        public string Name => "自定义占位触发器";  // 返回触发器的名称
        public bool IsEnabled { get; set; }  // 触发器是否启用的标志
        public int Priority => 9999;  // 触发器的优先级
        public bool IsExclusive { get; private set; }  // 触发器是否独占的标志

        private readonly Pen _pen = new(Color.Coral, 1);  // 定义用于绘图的画笔，颜色为珊瑚色，宽度为1

        //private readonly AutoGeniusInvokationAssets _autoGeniusInvokationAssets;  // 注释掉的字段，可能用于自动调用资产

        // private readonly YoloV8 _predictor = new(Global.Absolute("Assets\\Model\\Domain\\bgi_tree.onnx"));  // 注释掉的字段，可能用于模型预测

        public TestTrigger()
        {
            var info = TaskContext.Instance().SystemInfo;  // 获取任务上下文中的系统信息
            //_autoGeniusInvokationAssets = new AutoGeniusInvokationAssets();  // 注释掉的初始化代码
        }

        public void Init()
        {
            IsEnabled = false;  // 初始化时禁用触发器
            IsExclusive = false;  // 初始化时设置触发器为非独占
        }

        public void OnCapture(CaptureContent content)  // 方法的定义，处理捕获内容
        }

        // private void Detect(CaptureContent content)  // 注释掉的方法，可能用于检测捕获的内容
        // {
        //     using var memoryStream = new MemoryStream();  // 创建内存流对象
        //     content.CaptureRectArea.SrcBitmap.Save(memoryStream, ImageFormat.Bmp);  // 将捕获的图像保存到内存流
        //     memoryStream.Seek(0, SeekOrigin.Begin);  // 将内存流的位置重置到起始位置
        //     var result = _predictor.Detect(memoryStream);  // 使用预测器检测内存流中的图像
        //     Debug.WriteLine(result);  // 打印检测结果
        //     var list = new List<RectDrawable>();  // 创建矩形绘制对象列表
        //     foreach (var box in result.Boxes)  // 遍历检测到的框
        //     {
        //         var rect = new System.Windows.Rect(box.Bounds.X, box.Bounds.Y, box.Bounds.Width, box.Bounds.Height);  // 创建矩形对象
        //         list.Add(new RectDrawable(rect, _pen));  // 将矩形绘制对象添加到列表
        //     }
        //
        //     VisionContext.Instance().DrawContent.PutOrRemoveRectList("TreeBox", list);  // 更新视觉上下文中的矩形列表
        // }

        public static void TestArrow(CaptureContent content)  // 静态方法的定义，用于测试箭头
        }

        static Point Midpoint(Point p1, Point p2)  // 静态方法，计算两个点的中点
        {
            var midX = (p1.X + p2.X) / 2;  // 计算 X 坐标的中点
            var midY = (p1.Y + p2.Y) / 2;  // 计算 Y 坐标的中点
            return new Point(midX, midY);  // 返回中点坐标
        }

        public void TestCamera(CaptureContent content)  // 方法的定义，用于测试相机
        }
    }
}
    {
        # 从 SrcGreyMat 中裁剪出指定区域，创建新的 Mat 对象
        var mat = new Mat(content.CaptureRectArea.SrcGreyMat, new Rect(62, 19, 212, 212));
        # 对 mat 进行高斯模糊处理，内核大小为 3x3
        Cv2.GaussianBlur(mat, mat, new Size(3, 3), 0);
        # 极坐标展开
        var centerPoint = new Point2f(mat.Width / 2f, mat.Height / 2f);
        # 创建一个新的 Mat 对象用于存储极坐标展开结果
        var polarMat = new Mat();
        # 执行极坐标变换，将 mat 转换为极坐标表示的 polarMat
        Cv2.WarpPolar(mat, polarMat, new Size(360, 360), centerPoint, 360d, InterpolationFlags.Linear, WarpPolarMode.Linear);
        # 创建一个新的 Mat 对象，提取 polarMat 的特定区域
        var polarRoiMat = new Mat(polarMat, new Rect(10, 0, 70, polarMat.Height));
        # 将 polarRoiMat 顺时针旋转 90 度
        Cv2.Rotate(polarRoiMat, polarRoiMat, RotateFlags.Rotate90Counterclockwise);

        # 创建一个新的 Mat 对象用于存储 Scharr 算子结果
        var scharrResult = new Mat();
        # 对 polarRoiMat 执行 Scharr 边缘检测，结果存储在 scharrResult 中
        Cv2.Scharr(polarRoiMat, scharrResult, MatType.CV_32F, 1, 0);

        # 创建两个数组，用于存储波峰位置的统计
        var left = new int[360];
        var right = new int[360];

        # 将 scharrResult 转换为浮点数组
        scharrResult.GetArray<float>(out var array);
        # 寻找左半部分的波峰
        var leftPeaks = FindPeaks(array);
        leftPeaks.ForEach(i => left[i % 360]++);

        # 反转数组中的值，并寻找右半部分的波峰
        var reversedArray = array.Select(x => -x).ToArray();
        var rightPeaks = FindPeaks(reversedArray);
        rightPeaks.ForEach(i => right[i % 360]++);

        # 优化左侧和右侧波峰统计数据，计算它们的差值
        var left2 = left.Zip(right, (x, y) => Math.Max(x - y, 0)).ToArray();
        var right2 = right.Zip(left, (x, y) => Math.Max(x - y, 0)).ToArray();

        # 在邻近 2° 范围内，通过左移后相乘来寻找最大值
        var sum = new int[360];
        for (var i = -2; i <= 2; i++)
        {
            # 将右侧数组左移并与左侧数组逐元素相乘，计算加权和
            var all = left2.Zip(Shift(right2, -90 + i), (x, y) => x * y * (3 - Math.Abs(i)) / 3).ToArray();
            sum = sum.Zip(all, (x, y) => x + y).ToArray();
        }

        # 对结果数组进行卷积操作
        var result = new int[360];
        for (var i = -2; i <= 2; i++)
        {
            var all = Shift(sum, i);
            for (var j = 0; j < all.Length; j++)
            {
                all[j] = all[j] * (3 - Math.Abs(i)) / 3;
            }

            result = result.Zip(all, (x, y) => x + y).ToArray();
        }

        # 计算最大值的索引并将其转换为角度
        var maxIndex = result.ToList().IndexOf(result.Max());
        var angle = maxIndex + 45;

        const int r = 100;
        # 定义中心点坐标
        var center = new Point(168, 125);
        # 根据角度计算圆周上的点坐标
        var x1 = center.X + r * Math.Cos(angle * Math.PI / 180);
        var y1 = center.Y + r * Math.Sin(angle * Math.PI / 180);

        # 画线相关代码被注释掉了
        # var line = new LineDrawable(center, new Point(x1, y1));
        # line.Pen = new Pen(Color.Yellow, 1);
        # VisionContext.Instance().DrawContent.PutLine("camera", line);
    }

    # 寻找数组中的波峰
    static List<int> FindPeaks(float[] data)
    {
        List<int> peakIndices = [];

        for (int i = 1; i < data.Length - 1; i++)
        {
            # 判断当前位置是否为波峰
            if (data[i] > data[i - 1] && data[i] > data[i + 1])
            {
                peakIndices.Add(i);
            }
        }

        return peakIndices;
    }

    # 将数组右移 k 位
    public static int[] RightShift(int[] array, int k)
    {
        return array.Skip(array.Length - k)
            .Concat(array.Take(array.Length - k))
            .ToArray();
    }

    # 将数组左移 k 位
    public static int[] LeftShift(int[] array, int k)
    # 先跳过数组开头的 k 个元素，再将剩下的部分加上前 k 个元素，返回新的数组
    {
        return array.Skip(k)
            .Concat(array.Take(k))
            .ToArray();
    }

    # 定义静态方法 Shift，接收一个数组和一个整数 k 作为参数
    public static int[] Shift(int[] array, int k)
    {
        # 如果 k 大于 0，则调用右移方法
        if (k > 0)
        {
            return RightShift(array, k);
        }
        # 如果 k 小于或等于 0，则调用左移方法（k 取负值以适应左移）
        else
        {
            return LeftShift(array, -k);
        }
    }
# 结束当前代码块或控制结构
}
```