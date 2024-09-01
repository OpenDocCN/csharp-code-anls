# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\MiniMap\CameraOrientationTest.cs`

```cs
# 定义命名空间，包含相关的类和方法
﻿using OpenCvSharp;

namespace BetterGenshinImpact.Test.Simple.MiniMap;

# 定义一个名为 CameraOrientationTest 的公共类
public class CameraOrientationTest
{
    # 定义一个公共静态方法 Test1，返回三个整数数组的元组
    public static (int[], int[], int[]) Test1()
    {
        # 从指定路径读取图像文件，并以灰度模式加载
        var mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\视角朝向识别\3.png", ImreadModes.Grayscale);
        # 对图像应用高斯模糊，以减小噪声
        Cv2.GaussianBlur(mat, mat, new Size(3, 3), 0);
        # 定义极坐标转换的中心点，位于图像的中心
        var centerPoint = new Point2f(mat.Width / 2f, mat.Height / 2f);
        # 创建一个新矩阵用于存储极坐标结果
        var polarMat = new Mat();
        # 将图像进行极坐标变换，输出到 polarMat
        Cv2.WarpPolar(mat, polarMat, new Size(360, 360), centerPoint, 360d, InterpolationFlags.Linear, WarpPolarMode.Linear);
        # Cv2.ImShow("polarMat", polarMat);  // 显示极坐标矩阵（被注释掉）
        # 选取 polarMat 的一个区域，创建一个 ROI 矩阵
        var polarRoiMat = new Mat(polarMat, new Rect(10, 0, 70, polarMat.Height));
        # 将 ROI 矩阵顺时针旋转90度
        Cv2.Rotate(polarRoiMat, polarRoiMat, RotateFlags.Rotate90Counterclockwise);
        # 显示转换后的极坐标图像
        Cv2.ImShow("极坐标转换后", polarRoiMat);
        # Cv2.ImWrite(@"E:\HuiTask\更好的原神\自动秘境\视角朝向识别\2_p.png", polarRoiMat);  // 保存极坐标图像（被注释掉）

        # 创建一个矩阵用于存储 Scharr 算子结果
        var scharrResult = new Mat();
        # 对极坐标 ROI 矩阵应用 Scharr 算子以提取边缘
        Cv2.Scharr(polarRoiMat, scharrResult, MatType.CV_32F, 1, 0);

        # 创建一个矩阵用于显示的 Scharr 结果
        var scharrResultDisplay = new Mat();
        # 将 Scharr 算子结果转换为可显示的格式
        Cv2.ConvertScaleAbs(scharrResult, scharrResultDisplay);
        # 显示 Scharr 结果图像
        Cv2.ImShow("Scharr", scharrResultDisplay);

        # 创建两个整数数组用于存储波峰数据
        var left = new int[360];
        var right = new int[360];

        # 从 Scharr 结果中提取浮点数组
        scharrResult.GetArray<float>(out var array);
        # 寻找正向波峰并记录下标
        var leftPeaks = FindPeaks(array);
        # 更新左侧波峰计数
        leftPeaks.ForEach(i => left[i % 360]++);

        # 反转数组
        var reversedArray = array.Select(x => -x).ToArray();
        # 寻找反向波峰并记录下标
        var rightPeaks = FindPeaks(reversedArray);
        # 更新右侧波峰计数
        rightPeaks.ForEach(i => right[i % 360]++);

        # 计算优化后的左侧波峰数据
        var left2 = left.Zip(right, (x, y) => Math.Max(x - y, 0)).ToArray();
        # 计算优化后的右侧波峰数据
        var right2 = right.Zip(left, (x, y) => Math.Max(x - y, 0)).ToArray();

        # 对右侧波峰数据进行左移操作
        var rightAfterShift = Shift(right2, -90);
        # var all = left2.Zip(rightAfterShift, (x, y) => x * y).ToArray();  // 原本创建左右相乘数组的语句（被注释掉）

        # 在附近2°内求最大值的空数组
        var sum = new int[360];
        # 在[-2, 2]范围中进行循环
        for (var i = -2; i <= 2; i++)
        {
            # 对左侧优化数组和右移后数组进行相乘操作
            var all = left2.Zip(Shift(right2, -90 + i), (x, y) => x * y * (3 - Math.Abs(i)) / 3).ToArray();
            # 汇总结果
            sum = sum.Zip(all, (x, y) => x + y).ToArray();
        }

        # 创建一个用于卷积的结果数组
        var result = new int[360];
        # 在[-2, 2]范围中进行循环
        for (var i = -2; i <= 2; i++)
        {
            # 对汇总结果进行左移
            var all = Shift(sum, i);
            # 对每个元素进行加权处理
            for (var j = 0; j < all.Length; j++)
            {
                all[j] = all[j] * (3 - Math.Abs(i)) / 3;
            }
            # 累加卷积结果
            result = result.Zip(all, (x, y) => x + y).ToArray();
        }

        # 返回最终的左侧波峰数据、右侧波峰经过左移的数据，和卷积计算结果
        return (left2, rightAfterShift, result);
    }

    # static List<int> FindPeaks(double[] data)  // 寻找波峰的静态方法（被注释掉）
    # {  
    #     List<int> peakIndices = new List<int>();  // 存储波峰下标的列表（被注释掉）
    //    for (int i = 1; i < data.Length - 1; i++)
    //    {
    //        if (data[i] > data[i - 1] && data[i] > data[i + 1])
    //        {
    //            peakIndices.Add(i);
    //        }
    //    }

    //    return peakIndices;
    //}

    // 查找数据数组中的所有峰值索引
    static List<int> FindPeaks(float[] data)
    {
        // 初始化一个整数列表，用于存储峰值的索引
        List<int> peakIndices = new List<int>();

        // 遍历数据数组的每个元素，忽略第一个和最后一个元素
        for (int i = 1; i < data.Length - 1; i++)
        {
            // 如果当前元素大于前一个和后一个元素，则为峰值
            if (data[i] > data[i - 1] && data[i] > data[i + 1])
            {
                // 将峰值的索引添加到列表中
                peakIndices.Add(i);
            }
        }

        // 返回包含所有峰值索引的列表
        return peakIndices;
    }

    // 将数组向右移动 k 个位置
    public static int[] RightShift(int[] array, int k)
    {
        // 从数组末尾提取 k 个元素，并将剩余元素添加到前面
        return array.Skip(array.Length - k)
            .Concat(array.Take(array.Length - k))
            .ToArray();
    }

    // 将数组向左移动 k 个位置
    public static int[] LeftShift(int[] array, int k)
    {
        // 从数组中提取前 k 个元素，并将其添加到末尾
        return array.Skip(k)
            .Concat(array.Take(k))
            .ToArray();
    }

    // 根据 k 的符号选择左移或右移
    public static int[] Shift(int[] array, int k)
    {
        // 如果 k 为正，调用右移函数；否则，调用左移函数并传入 -k
        if (k > 0)
        {
            return RightShift(array, k);
        }
        else
        {
            return LeftShift(array, -k);
        }
    }

    // 测试极坐标变换及其效果
    public static void Test2()
    {
        // 读取图像文件到矩阵中
        var mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\视角朝向识别\1.png", ImreadModes.Color);

        // 极坐标展开
        var centerPoint = new Point2f(mat.Width / 2f, mat.Height / 2f);
        var polarMat = new Mat();
        // 对图像进行极坐标变换
        Cv2.WarpPolar(mat, polarMat, new Size(360, 360), centerPoint, 360d, InterpolationFlags.Linear, WarpPolarMode.Linear);
        var polarRoiMat = new Mat(polarMat, new Rect(20, 0, 70, polarMat.Height));
        // 旋转图像 90 度
        Cv2.Rotate(polarRoiMat, polarRoiMat, RotateFlags.Rotate90Counterclockwise);
        // 将图像从 BGR 转换为 Lab 颜色空间
        Cv2.CvtColor(polarRoiMat, polarRoiMat, ColorConversionCodes.BGR2Lab);
        // 显示极坐标转换后的图像
        Cv2.ImShow("极坐标转换后", polarRoiMat);

        // 分离图像的各个通道
        var splitMat = polarRoiMat.Split();

        // 显示每个通道图像
        for (int i = 0; i < splitMat.Length; i++)
        {
            Cv2.ImShow($"splitMat{i}", splitMat[i]);
        }

        var andMat = new Mat();
        // 对第一个通道应用 Scharr 边缘检测
        Cv2.Scharr(splitMat[0], andMat, MatType.CV_8UC1, 1, 0);
        // 显示边缘检测后的图像
        Cv2.ImShow("Scharr", andMat);
    }
# 结束了当前的代码块或控制结构
}
```