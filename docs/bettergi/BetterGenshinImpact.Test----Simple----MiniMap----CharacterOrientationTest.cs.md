# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\MiniMap\CharacterOrientationTest.cs`

```cs
# 引入系统诊断命名空间，用于调试
﻿using System.Diagnostics;
# 引入 BetterGenshinImpact 项目中的 OpenCv 相关功能
using BetterGenshinImpact.Core.Recognition.OpenCv;
# 引入 OpenCvSharp 库的功能
using OpenCvSharp;

# 定义命名空间 BetterGenshinImpact.Test.Simple.MiniMap
namespace BetterGenshinImpact.Test.Simple.MiniMap;

# 定义一个名为 CharacterOrientationTest 的公共类
public class CharacterOrientationTest
{
    # 定义一个公共静态方法 TestArrow
    public static void TestArrow()
    {
        # 从指定路径读取图像文件，使用彩色模式读取
        var mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\箭头识别\3.png", ImreadModes.Color);
        # 定义低阈值颜色范围的标量
        var lowScalar = new Scalar(0, 207, 255);
        # 定义高阈值颜色范围的标量
        var highScalar = new Scalar(0, 208, 255);
        # 使用颜色阈值过滤图像，返回处理后的灰度图像
        var gray = OpenCvCommonHelper.Threshold(mat, lowScalar, highScalar);
        # 显示处理后的灰度图像，窗口标题为 "gray"
        Cv2.ImShow("gray", gray);
    }

    # 计算两个点之间的欧氏距离，点的坐标使用整数表示
    static double Distance(Point pt1, Point pt2)
    {
        # 计算两个点在 x 轴上的差值的绝对值
        int deltaX = Math.Abs(pt2.X - pt1.X);
        # 计算两个点在 y 轴上的差值的绝对值
        int deltaY = Math.Abs(pt2.Y - pt1.Y);

        # 使用勾股定理计算距离
        return Math.Sqrt(deltaX * deltaX + deltaY * deltaY);
    }

    # 计算两个点之间的欧氏距离，点的坐标使用浮点数表示
    static double Distance(Point2f pt1, Point2f pt2)
    {
        # 计算两个点在 x 轴上的差值的绝对值
        var deltaX = Math.Abs(pt2.X - pt1.X);
        # 计算两个点在 y 轴上的差值的绝对值
        var deltaY = Math.Abs(pt2.Y - pt1.Y);

        # 使用勾股定理计算距离
        return Math.Sqrt(deltaX * deltaX + deltaY * deltaY);
    }

    # 注释掉的代码：计算两个 Point2f 类型点的中点
    // static Point2f Midpoint(Point2f p1, Point2f p2)
    // {
    //     # 计算中点的 x 坐标
    //     var midX = (p1.X + p2.X) / 2;
    //     # 计算中点的 y 坐标
    //     var midY = (p1.Y + p2.Y) / 2;
    //     # 返回中点的 Point2f 对象
    //     return new Point2f(midX, midY);
    // }

    # 计算两个 Point 类型点的中点
    static Point Midpoint(Point p1, Point p2)
    {
        # 计算中点的 x 坐标
        var midX = (p1.X + p2.X) / 2;
        # 计算中点的 y 坐标
        var midY = (p1.Y + p2.Y) / 2;
        # 返回中点的 Point 对象
        return new Point(midX, midY);
    }

    # 定义一个公共静态方法 Triangle，参数包括源图像和灰度图像（尚未实现）
    public static void Triangle(Mat src, Mat gray)
    # 查找图像中所有的轮廓，返回轮廓和层级信息
    Cv2.FindContours(gray, out var contours, out var hierarchy, RetrievalModes.External, ContourApproximationModes.ApproxNone);
    # 创建一个与输入图像大小相同的全黑图像
    Mat dst = Mat.Zeros(gray.Size(), MatType.CV_8UC3);
    # 遍历所有检测到的轮廓
    for (int i = 0; i < contours.Length; i++)
    {
        # 在输入图像上绘制每个轮廓，轮廓颜色为红色，线宽为1
        Cv2.DrawContours(src, contours, i, Scalar.Red, 1, LineTypes.Link4, hierarchy);
    }
    # 显示包含所有轮廓的图像（被注释掉了）
    // Cv2.ImShow("目标", dst);

    # 创建一个与输入图像大小相同的全黑图像
    Mat dst2 = Mat.Zeros(gray.Size(), MatType.CV_8UC3);
    # 遍历所有检测到的轮廓
    for (int i = 0; i < contours.Length; i++)
    {
        # 计算轮廓的周长
        double perimeter = Cv2.ArcLength(contours[i], true);

        # 近似轮廓为多边形
        Point[] approx = Cv2.ApproxPolyDP(contours[i], 0.04 * perimeter, true);

        # 如果近似的多边形有三个顶点，认为是三角形
        if (approx.Length == 3)
        {
            # 在输入图像上绘制三角形的轮廓，轮廓颜色为绿色，线宽为1
            Cv2.DrawContours(src, new Point[][] { approx }, -1, Scalar.Green, 1);
            # 计算三角形三条边的长度
            var sideLengths = new double[3];
            sideLengths[0] = Distance(approx[1], approx[2]);
            sideLengths[1] = Distance(approx[2], approx[0]);
            sideLengths[2] = Distance(approx[0], approx[1]);

            # 找到最短边
            var result = sideLengths
                .Select((value, index) => new { Value = value, Index = index })
                .OrderBy(item => item.Value)
                .First();

            # 计算最短边的中点
            var residue = approx.ToList();
            residue.RemoveAt(result.Index);
            var midPoint = new Point((residue[0].X + residue[1].X) / 2, (residue[0].Y + residue[1].Y) / 2);

            # 在图像上绘制从中点到最短边的延长线，线的颜色为红色，线宽为1
            Cv2.Line(src, midPoint, approx[result.Index] + (approx[result.Index] - midPoint) * 3, Scalar.Red, 1);

            # 输出计算的角度
            Debug.WriteLine(CalculateAngle(midPoint, approx[result.Index]));
        }
    }

    # 显示包含所有绘制标记的图像
    Cv2.ImShow("目标2", src);


    # 读取图像，指定读取模式为彩色
    public static void TestArrow2()
    {
        var mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\箭头识别\s1.png", ImreadModes.Color);
        # 对图像进行高斯模糊，减少噪声
        Cv2.GaussianBlur(mat, mat, new Size(3, 3), 0);
        # 将图像拆分为三个颜色通道
        var splitMat = mat.Split();

        # 红蓝通道按位与操作，提取特定颜色区域
        var red = new Mat(mat.Size(), MatType.CV_8UC1);
        Cv2.InRange(splitMat[0], new Scalar(250), new Scalar(255), red);
        # 显示提取的红色区域（被注释掉了）
        //Cv2.ImShow("red", red);
        var blue = new Mat(mat.Size(), MatType.CV_8UC1);
        Cv2.InRange(splitMat[2], new Scalar(0), new Scalar(10), blue);
        # 显示提取的蓝色区域（被注释掉了）
        //Cv2.ImShow("blue", blue);
        # 计算红色和蓝色区域的按位与结果
        var andMat = red & blue;
        # 显示按位与结果图像
        Cv2.ImShow("andMat2", andMat);
        # 调用 Triangle 方法处理提取的区域
        Triangle(mat, andMat);
    }
    {
        # 读取图像文件并将其加载到一个 Mat 对象中，图像模式为彩色
        var mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\箭头识别\s1.png", ImreadModes.Color);
        # 对图像应用高斯模糊，以减少噪声
        Cv2.GaussianBlur(mat, mat, new Size(3, 3), 0);
        # 将图像分成三个通道：B、G 和 R
        var splitMat = mat.Split();

        //for (int i = 0; i < splitMat.Length; i++)
        //{
        //    Cv2.ImShow($"splitMat{i}", splitMat[i]);
        //}

        # 创建一个空的 Mat 对象用于存储红色通道的二值化结果
        var red = new Mat(mat.Size(), MatType.CV_8UC1);
        # 将红色通道的像素值范围设为 255，将符合条件的像素标记为白色（255）
        Cv2.InRange(splitMat[0], new Scalar(255), new Scalar(255), red);
        //Cv2.ImShow("red", red);
        # 创建一个空的 Mat 对象用于存储蓝色通道的二值化结果
        var blue = new Mat(mat.Size(), MatType.CV_8UC1);
        # 将蓝色通道的像素值范围设为 0，将符合条件的像素标记为白色（255）
        Cv2.InRange(splitMat[2], new Scalar(0), new Scalar(0), blue);
        //Cv2.ImShow("blue", blue);
        # 对红色和蓝色通道的结果进行按位与操作
        var andMat = red & blue;
        // Cv2.ImShow("andMat", andMat);

        # 创建结构元素用于形态学操作
        var kernel = Cv2.GetStructuringElement(MorphShapes.Rect, new Size(3, 3));
        # 创建一个空的 Mat 对象用于存储腐蚀操作的结果
        var res = new Mat(mat.Size(), MatType.CV_8UC1);
        # 对按位与结果进行腐蚀操作，减少噪声
        Cv2.Erode(andMat.ToMat(), res, kernel);
        # 显示腐蚀后的图像
        Cv2.ImShow("erode", res);

        # 膨胀操作被注释掉了
        // var res2 = new Mat(mat.Size(), MatType.CV_8UC1);
        // Cv2.Dilate(res, res2, kernel);
        // Cv2.ImShow("dilate", res2);

        # 查找图像中的轮廓
        Cv2.FindContours(res, out var contours, out var hierarchy, RetrievalModes.External, ContourApproximationModes.ApproxNone);
        # 创建一个空的 Mat 对象用于绘制检测到的矩形
        Mat dst = Mat.Zeros(res.Size(), MatType.CV_8UC3);
        # 遍历每个轮廓
        for (int i = 0; i < contours.Length; i++)
        {
            # 计算最小外接矩形
            var rect = Cv2.MinAreaRect(contours[i]);
            # 获取矩形的四个顶点
            var points = Cv2.BoxPoints(rect);
            # 绘制矩形的边
            for (int j = 0; j < 4; ++j)
            {
                Cv2.Line(mat, (Point)points[j], (Point)points[(j + 1) % 4], Scalar.Red, 1);
            }

            # 计算矩形的角度并输出
            if (Distance(points[0], points[1]) > Distance(points[1], points[2]))
            {
                Debug.WriteLine(CalculateAngle(points[0], points[1]));
            }
            else
            {
                Debug.WriteLine(CalculateAngle(points[1], points[2]));
            }
        }

        # 显示带有矩形标记的原始图像
        Cv2.ImShow("目标", mat);

        # 调用测试函数
        TestArrow2();
    }

    # 计算两点之间的角度
    static double CalculateAngle(Point2f point1, Point2f point2)
    {
        # 计算两点之间的角度（弧度）
        var angleRadians = Math.Atan2(point2.Y - point1.Y, point2.X - point1.X);
        # 将弧度转换为角度
        var angleDegrees = angleRadians * (180 / Math.PI);

        # 如果角度为负值，调整为正值
        if (angleDegrees < 0)
        {
            angleDegrees += 360;
        }

        # 返回角度
        return angleDegrees;
    }

    # 蚁填充函数（尚未实现）
    public static void FloodFill()
    {
        # 从指定路径读取图像，转换为彩色图像矩阵
        var mat = Cv2.ImRead(@"E:\HuiTask\更好的原神\自动秘境\箭头识别\s1.png", ImreadModes.Color);
        # 对图像应用高斯模糊，减少噪声
        Cv2.GaussianBlur(mat, mat, new Size(3, 3), 0);
        # 将图像矩阵分割为颜色通道
        var splitMat = mat.Split();

        //for (int i = 0; i < splitMat.Length; i++)
        //{
        //    # 显示分割后的每个颜色通道
        //    Cv2.ImShow($"splitMat{i}", splitMat[i]);
        //}

        # 1. 对红色和蓝色通道进行按位与操作
        # 创建一个与原图像大小相同的单通道矩阵用于存储红色部分
        var red = new Mat(mat.Size(), MatType.CV_8UC1);
        # 在红色通道中进行阈值处理，提取红色部分
        Cv2.InRange(splitMat[0], new Scalar(250), new Scalar(255), red);
        #Cv2.ImShow("red", red);
        # 创建一个与原图像大小相同的单通道矩阵用于存储蓝色部分
        var blue = new Mat(mat.Size(), MatType.CV_8UC1);
        # 在蓝色通道中进行阈值处理，提取蓝色部分
        Cv2.InRange(splitMat[2], new Scalar(0), new Scalar(10), blue);
        #Cv2.ImShow("blue", blue);
        # 创建一个用于存储红色和蓝色按位与结果的矩阵
        var andMat = new Mat(mat.Size(), MatType.CV_8UC1);

        # 对红色和蓝色部分进行按位与操作，保留两者共同存在的部分
        Cv2.BitwiseAnd(red, blue, andMat);
        # 显示按位与操作的结果
        Cv2.ImShow("andMat2", andMat);

        # 寻找图像中的轮廓
        Cv2.FindContours(andMat, out var contours, out var hierarchy, RetrievalModes.External, ContourApproximationModes.ApproxSimple);
        # 创建一个与原图像大小相同的三通道矩阵，用于绘制轮廓
        Mat dst = Mat.Zeros(andMat.Size(), MatType.CV_8UC3);
        # 遍历找到的轮廓，并在目标图像上绘制
        for (int i = 0; i < contours.Length; i++)
        {
            Cv2.DrawContours(dst, contours, i, Scalar.Red, 1, LineTypes.Link4, hierarchy);
        }

        # 显示绘制轮廓后的图像
        Cv2.ImShow("寻找轮廓", dst);

        # 计算最大外接矩形
        if (contours.Length > 0)
        {
            # 获取每个轮廓的外接矩形，并过滤掉高度小于2的矩形
            var boxes = contours.Select(Cv2.BoundingRect).Where(w => w.Height >= 2);
            var boxArray = boxes as Rect[] ?? boxes.ToArray();
            # 如果找到的外接矩形不等于1个，则抛出异常
            if (boxArray.Count() != 1)
            {
                throw new Exception("找到多个外接矩形");
            }

            # 选择第一个（也是唯一的）外接矩形
            var box = boxArray.First();

            # 剪裁出外接矩形区域，并将区域放大4倍
            var newSrcMat = new Mat(mat, new Rect(box.X - box.Width / 2, box.Y - box.Height / 2, box.Width * 2, box.Height * 2));
            # 显示剪裁出的区域
            Cv2.ImShow("剪裁出准备泛洪的区域", newSrcMat);

            # 使用区域中心点作为种子点进行泛洪填充
            var seedPoint = new Point(newSrcMat.Width / 2, newSrcMat.Height / 2);

            # 在剪裁区域内进行泛洪填充，填充颜色为白色
            Cv2.FloodFill(newSrcMat, seedPoint, Scalar.White, out _, new Scalar());
        }
    }

    # 定义一个静态方法 Hsv（该方法尚未实现）
    public static void Hsv()
    }
你给出的代码是一个闭合的右花括号 `}`。请提供更多代码或上下文，这样我可以为你提供准确的注释。
```