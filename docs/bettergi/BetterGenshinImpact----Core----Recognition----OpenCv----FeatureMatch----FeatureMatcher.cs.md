# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\FeatureMatch\FeatureMatcher.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OpenCv.Model; // 引用自定义的 OpenCv 模型相关命名空间
using BetterGenshinImpact.GameTask.Common.Map; // 引用游戏地图相关命名空间
using BetterGenshinImpact.Helpers; // 引用帮助工具命名空间
using OpenCvSharp; // 引用 OpenCvSharp 库
using OpenCvSharp.Features2D; // 引用 OpenCvSharp 特征检测相关类
using OpenCvSharp.XFeatures2D; // 引用 OpenCvSharp 扩展特征检测类
using System; // 引用系统基础功能命名空间
using System.Collections.Generic; // 引用系统集合命名空间
using System.Diagnostics; // 引用系统调试功能命名空间
using System.IO; // 引用系统输入输出功能命名空间
using System.Linq; // 引用系统 LINQ 查询功能命名空间

namespace BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch; // 定义命名空间

public class FeatureMatcher // 定义 FeatureMatcher 类
{
    private readonly double _threshold = 100; // SURF 100 // 定义 SURF 特征匹配的阈值
    private readonly Feature2D _feature2D; // 定义 Feature2D 对象，用于特征检测

    private readonly Dictionary<DescriptorMatcherType, DescriptorMatcher> _matcherFactory = new() // 定义描述符匹配器工厂字典
    {
        { DescriptorMatcherType.BruteForce, DescriptorMatcher.Create(DescriptorMatcherType.BruteForce.ToString()) }, // 创建暴力匹配器
        { DescriptorMatcherType.FlannBased, DescriptorMatcher.Create(DescriptorMatcherType.FlannBased.ToString()) } // 创建 FLANN 匹配器
    };

    private readonly Size _trainMatSize; // 大图大小 // 定义训练图像的大小

    private readonly Mat _trainDescriptors = new(); // 大图特征描述子 // 定义训练图像的特征描述子矩阵
    private readonly KeyPoint[] _trainKeyPoints; // 定义训练图像的关键点数组

    private readonly KeyPointFeatureBlock[][] _blocks; // 特征块存储 // 定义特征块的二维数组
    private readonly int _splitRow = MapCoordinate.GameMapRows * 2; // 特征点拆分行数 // 定义特征点拆分的行数
    private readonly int _splitCol = MapCoordinate.GameMapCols * 2; // 特征点拆分列数 // 定义特征点拆分的列数
    private KeyPointFeatureBlock? _lastMergedBlock; // 上次合并的特征块 // 定义上一次合并的特征块

    /// <summary>
    /// 从图像 or 特征点加载
    /// 大图不建议使用此构造函数加载，速度很慢
    /// </summary>
    /// <param name="trainMat"></param>
    /// <param name="featureStorage"></param>
    /// <param name="type"></param>
    /// <exception cref="Exception"></exception>
    public FeatureMatcher(Mat trainMat, FeatureStorage? featureStorage = null, Feature2DType type = Feature2DType.SIFT) // FeatureMatcher 构造函数
    {
        // 获取训练图像矩阵的尺寸
        _trainMatSize = trainMat.Size();
        // 根据特征类型选择使用 SURF 还是 SIFT
        if (Feature2DType.SURF == type)
        {
            // 创建 SURF 特征提取器
            _feature2D = SURF.Create(_threshold, 4, 3, false, true);
        }
        else
        {
            // 创建 SIFT 特征提取器
            _feature2D = SIFT.Create();
        }
    
        // 如果特征存储对象不为空
        if (featureStorage != null)
        {
            // 设置特征存储的类型名称
            featureStorage.TypeName = type.ToString();
            // 输出调试信息，尝试从磁盘加载特征点
            Debug.WriteLine("尝试从磁盘加载特征点");
            // 从磁盘加载特征点数组
            var kpFromDisk = featureStorage.LoadKeyPointArray();
            // 如果特征点数组为空
            if (kpFromDisk == null)
            {
                // 输出调试信息，特征点不存在
                Debug.WriteLine("特征点不存在");
                // 检测并计算特征点和描述符
                _feature2D.DetectAndCompute(trainMat, null, out _trainKeyPoints, _trainDescriptors);
                // 保存特征点和描述符到磁盘
                featureStorage.SaveKeyPointArray(_trainKeyPoints);
                featureStorage.SaveDescMat(_trainDescriptors);
            }
            else
            {
                // 从磁盘加载的特征点数组赋值
                _trainKeyPoints = kpFromDisk;
                // 从磁盘加载的特征描述矩阵赋值，如果为空则抛出异常
                _trainDescriptors = featureStorage.LoadDescMat() ?? throw new Exception("加载特征描述矩阵失败");
            }
        }
        else
        {
            // 如果没有特征存储对象，则直接检测和计算特征点和描述符
            _feature2D.DetectAndCompute(trainMat, null, out _trainKeyPoints, _trainDescriptors);
        }
    
        // 输出调试信息，初始化 KeyPoint 完成
        Debug.WriteLine("被匹配的图像生成初始化KeyPoint完成");
        // 创建计时器并启动
        Stopwatch sw = new();
        sw.Start();
        // 切割特征点
        _blocks = KeyPointFeatureBlockHelper.SplitFeatures(_trainMatSize, _splitRow, _splitCol, _trainKeyPoints, _trainDescriptors);
        // 停止计时
        sw.Stop();
        // 输出调试信息，特征点切割耗时
        Debug.WriteLine($"切割特征点耗时: {sw.ElapsedMilliseconds}ms");
    }
    
    /// <summary>
    /// 直接从特征点加载
    /// </summary>
    /// <param name="trainMatSize"></param>
    /// <param name="featureStorage"></param>
    /// <param name="type"></param>
    /// <exception cref="Exception"></exception>
    public FeatureMatcher(Size trainMatSize, FeatureStorage featureStorage, Feature2DType type = Feature2DType.SIFT)
    {
        // 初始化训练图像矩阵的尺寸
        _trainMatSize = trainMatSize;
        // 根据特征类型选择使用 SURF 还是 SIFT
        if (Feature2DType.SURF == type)
        {
            // 创建 SURF 特征提取器
            _feature2D = SURF.Create(_threshold, 4, 3, false, true);
        }
        else
        {
            // 创建 SIFT 特征提取器
            _feature2D = SIFT.Create();
        }
    
        // 设置特征存储的类型名称
        featureStorage.TypeName = type.ToString();
        // 输出调试信息，尝试从磁盘加载特征点
        Debug.WriteLine("尝试从磁盘加载特征点");
        // 从磁盘加载特征点数组，如果为空则抛出异常
        _trainKeyPoints = featureStorage.LoadKeyPointArray() ?? throw new Exception("特征点不存在");
        // 从磁盘加载特征描述矩阵，如果为空则抛出异常
        _trainDescriptors = featureStorage.LoadDescMat() ?? throw new Exception("加载特征描述矩阵失败");
    
        // 输出调试信息，初始化 KeyPoint 完成
        Debug.WriteLine("被匹配的图像生成初始化KeyPoint完成");
        // 创建计时器并启动
        Stopwatch sw = new();
        sw.Start();
        // 切割特征点
        _blocks = KeyPointFeatureBlockHelper.SplitFeatures(_trainMatSize, _splitRow, _splitCol, _trainKeyPoints, _trainDescriptors);
        // 停止计时
        sw.Stop();
        // 输出调试信息，特征点切割耗时
        Debug.WriteLine($"切割特征点耗时: {sw.ElapsedMilliseconds}ms");
    }
    
    public DescriptorMatcher GetMatcher(DescriptorMatcherType type)
    {
        // 根据类型从工厂获取描述符匹配器
        return _matcherFactory[type];
    }
    
    #region 普通匹配
    
    /// <summary>
    /// 普通匹配（全图特征）
    /// </summary>
    /// <param name="queryMat"></param>
    /// <param name="queryMatMask"></param>
    /// <returns></returns>
    // 匹配查询图像特征与训练特征点，使用默认的特征点和描述符
    public Point2f Match(Mat queryMat, Mat? queryMatMask = null)
    {
        // 调用重载的 Match 方法，传入训练特征点和描述符
        return Match(_trainKeyPoints, _trainDescriptors, queryMat, queryMatMask);
    }

    /// <summary>
    /// 合并邻近的特征点后匹配（临近特征）
    /// </summary>
    /// <param name="queryMat">查询的图</param>
    /// <param name="prevX">上次匹配到的坐标x</param>
    /// <param name="prevY">上次匹配到的坐标y</param>
    /// <param name="queryMatMask">查询Mask</param>
    /// <returns></returns>
    public Point2f Match(Mat queryMat, float prevX, float prevY, Mat? queryMatMask = null)
    {
        // 获取上次匹配点所在的特征块的行列索引
        var (cellRow, cellCol) = KeyPointFeatureBlockHelper.GetCellIndex(_trainMatSize, _splitRow, _splitCol, prevX, prevY);
        // 输出当前坐标及其所在特征块的信息
        Debug.WriteLine($"当前坐标({prevX},{prevY})在特征块({cellRow},{cellCol})中");
        // 如果上次合并的特征块为空，或者不在当前特征块中，则需要重新合并特征点
        if (_lastMergedBlock == null || _lastMergedBlock.MergedCenterCellRow != cellRow || _lastMergedBlock.MergedCenterCellCol != cellCol)
        {
            // 输出切换到新的特征块的调试信息
            Debug.WriteLine($"---------切换到新的特征块({cellRow},{cellCol})，合并特征点--------");
            // 合并当前特征块的邻近特征点
            _lastMergedBlock = KeyPointFeatureBlockHelper.MergeNeighboringFeatures(_blocks, _trainDescriptors, cellRow, cellCol);
        }

        // 使用合并后的特征点和描述符进行匹配
        return Match(_lastMergedBlock.KeyPointArray, _lastMergedBlock.Descriptor!, queryMat, queryMatMask);
    }

    /// <summary>
    /// 普通匹配
    /// </summary>
    /// <param name="trainKeyPoints"></param>
    /// <param name="trainDescriptors"></param>
    /// <param name="queryMat"></param>
    /// <param name="queryMatMask"></param>
    /// <param name="matcherType"></param>
    /// <returns></returns>
    public Point2f Match(KeyPoint[] trainKeyPoints, Mat trainDescriptors, Mat queryMat, Mat? queryMatMask = null,
        DescriptorMatcherType matcherType = DescriptorMatcherType.FlannBased)
    {
        // 创建一个计时器，用于计算匹配的时间
        SpeedTimer speedTimer = new();

        // 使用 Mat 类来创建一个用于存储查询描述符的对象
        using var queryDescriptors = new Mat();
#pragma warning disable CS8604 // Disable warning about possible null reference for type parameters.
        _feature2D.DetectAndCompute(queryMat, queryMatMask, out var queryKeyPoints, queryDescriptors);
#pragma warning restore CS8604 // Re-enable warning about possible null reference for type parameters.
        // Record the time taken for keypoint generation
        speedTimer.Record("模板生成KeyPoint");

        // Match descriptors between query and training data
        var matches = GetMatcher(matcherType).Match(queryDescriptors, trainDescriptors);
        //Finding the Minimum and Maximum Distance
        double minDistance = 1000; // Initialize minimum distance for comparison
        double maxDistance = 0; // Initialize maximum distance for comparison
        for (int i = 0; i < queryDescriptors.Rows; i++)
        {
            // Retrieve distance for current match
            double distance = matches[i].Distance;
            // Update maximum distance if current distance is greater
            if (distance > maxDistance)
            {
                maxDistance = distance;
            }

            // Update minimum distance if current distance is smaller
            if (distance < minDistance)
            {
                minDistance = distance;
            }
        }

        // Debug.WriteLine($"max distance : {maxDistance}");
        // Debug.WriteLine($"min distance : {minDistance}");

        // Lists to hold filtered key points
        var pointsQuery = new List<Point2f>();
        var pointsTrain = new List<Point2f>();
        //Screening better matching points
        // var goodMatches = new List<DMatch>();
        for (int i = 0; i < queryDescriptors.Rows; i++)
        {
            // Retrieve distance for current match
            double distance = matches[i].Distance;
            // Add points if distance is within a reasonable range
            if (distance < Math.Max(minDistance * 2, 0.02))
            {
                pointsQuery.Add(queryKeyPoints[matches[i].QueryIdx].Pt);
                pointsTrain.Add(trainKeyPoints[matches[i].TrainIdx].Pt);
                //Compression of new ones with distances less than ranges DMatch
                // goodMatches.Add(matches[i]);
            }
        }

        // Record the time taken for matching process
        speedTimer.Record("FlannMatch");

        // var outMat = new Mat();

        // algorithm RANSAC Filter the matched results
        var pQuery = pointsQuery.ToPoint2d();
        var pTrain = pointsTrain.ToPoint2d();
        var outMask = new Mat();
        // If there are points available, perform homography computation
        if (pQuery.Count > 0 && pTrain.Count > 0)
        {
            // Compute homography using RANSAC to filter matches
            var hMat = Cv2.FindHomography(pQuery, pTrain, HomographyMethods.Ransac, mask: outMask);
            speedTimer.Record("FindHomography");

            // 1. 计算查询图像的中心点
            var queryCenterPoint = new Point2f(queryMat.Cols / 2f, queryMat.Rows / 2f);

            // 2. 使用单应矩阵进行透视变换
            Point2f[] queryCenterPoints = [queryCenterPoint];
            Point2f[] transformedCenterPoints = Cv2.PerspectiveTransform(queryCenterPoints, hMat);

            // 3. 获取变换后的中心点
            var trainCenterPoint = transformedCenterPoints[0];
            speedTimer.Record("PerspectiveTransform");
            speedTimer.DebugPrint();
            return trainCenterPoint;
        }

        speedTimer.DebugPrint();
        // Return a default point if no valid matches were found
        return new Point2f();
    }

    /// <summary>
    /// 普通匹配
    /// </summary>
    /// <param name="trainKeyPoints"></param>
    /// <param name="trainDescriptors"></param>
    /// <param name="queryMat"></param>
    /// <param name="queryMatMask"></param>
    /// <param name="matcherType"></param>
    /// <returns></returns>
    // 定义一个公开的方法 MatchCorners，返回类型为 Point2f 数组
    public Point2f[] MatchCorners(KeyPoint[] trainKeyPoints, Mat trainDescriptors, Mat queryMat, Mat? queryMatMask = null,
        DescriptorMatcherType matcherType = DescriptorMatcherType.FlannBased)
    {
        // 创建一个 SpeedTimer 实例，用于计时
        SpeedTimer speedTimer = new();

        // 创建一个 Mat 对象 queryDescriptors，用于存储查询描述符
        using var queryDescriptors = new Mat();
#pragma warning disable CS8604 // Disable warning about possible null reference for reference type parameters.
        _feature2D.DetectAndCompute(queryMat, queryMatMask, out var queryKeyPoints, queryDescriptors); 
        // Detect key points and compute descriptors for the query image

#pragma warning restore CS8604 // Re-enable the warning after the detection and computation
        speedTimer.Record("模板生成KeyPoint"); 
        // Record the time taken to generate key points

        var matches = GetMatcher(matcherType).Match(queryDescriptors, trainDescriptors); 
        // Match query descriptors against training descriptors based on matcher type

        double minDistance = 1000; //Backward approximation
        // Initialize minimum distance for later comparison
        double maxDistance = 0; 
        // Initialize maximum distance for later comparison

        for (int i = 0; i < queryDescriptors.Rows; i++) 
        // Iterate through each descriptor in the query set
        {
            double distance = matches[i].Distance; 
            // Get the distance of the current match

            if (distance > maxDistance) 
            // Update maximum distance if current distance is greater
            {
                maxDistance = distance; 
            }

            if (distance < minDistance) 
            // Update minimum distance if current distance is smaller
            {
                minDistance = distance; 
            }
        }

        // Debug.WriteLine($"max distance : {maxDistance}"); 
        // Debug.WriteLine($"min distance : {minDistance}"); 

        var pointsQuery = new List<Point2f>(); 
        // Initialize a list to hold key points from the query image
        var pointsTrain = new List<Point2f>(); 
        // Initialize a list to hold key points from the training image

        for (int i = 0; i < queryDescriptors.Rows; i++) 
        // Iterate through each descriptor in the query set
        {
            double distance = matches[i].Distance; 
            // Get the distance of the current match

            if (distance < Math.Max(minDistance * 2, 0.02)) 
            // Select matches with distance less than a threshold
            {
                pointsQuery.Add(queryKeyPoints[matches[i].QueryIdx].Pt); 
                // Add the key point from the query image to the list
                pointsTrain.Add(trainKeyPoints[matches[i].TrainIdx].Pt); 
                // Add the corresponding key point from the training image to the list
            }
        }

        speedTimer.Record("FlannMatch"); 
        // Record the time taken for FLANN-based matching

        var pQuery = pointsQuery.ToPoint2d(); 
        // Convert query points to Point2d format
        var pTrain = pointsTrain.ToPoint2d(); 
        // Convert training points to Point2d format
        var outMask = new Mat(); 
        // Initialize a matrix to hold the mask for RANSAC

        if (pQuery.Count > 0 && pTrain.Count > 0) 
        // Check if there are points to process
        {
            var hMat = Cv2.FindHomography(pQuery, pTrain, HomographyMethods.Ransac, mask: outMask); 
            // Compute the homography matrix using RANSAC algorithm
            speedTimer.Record("FindHomography"); 
            // Record the time taken to find the homography matrix

            var objCorners = new Point2f[4]; 
            // Initialize array to hold the corners of the query image
            objCorners[0] = new Point2f(0, 0); 
            // Top-left corner
            objCorners[1] = new Point2f(0, queryMat.Rows); 
            // Bottom-left corner
            objCorners[2] = new Point2f(queryMat.Cols, queryMat.Rows); 
            // Bottom-right corner
            objCorners[3] = new Point2f(queryMat.Cols, 0); 
            // Top-right corner

            var sceneCorners = Cv2.PerspectiveTransform(objCorners, hMat); 
            // Transform the corners using the computed homography matrix
            speedTimer.Record("PerspectiveTransform"); 
            // Record the time taken for perspective transformation
            speedTimer.DebugPrint(); 
            // Print debugging information
            return sceneCorners; 
            // Return the transformed corners
        }

        speedTimer.DebugPrint(); 
        // Print debugging information if no points to process
        return []; 
        // Return an empty array if no valid transformation
    }

    public Rect MatchRect(Mat queryMat, Mat? queryMatMask = null) 
    // Method to match a rectangle in the query image
    # 调用 MatchCorners 方法获取匹配的角点
    var corners = MatchCorners(_trainKeyPoints, _trainDescriptors, queryMat, queryMatMask);
    # 如果没有找到匹配的角点，返回一个空矩形
    if (corners.Length == 0)
    {
        return Rect.Empty;
    }
    # 根据匹配的角点计算并返回包围这些角点的矩形
    return Cv2.BoundingRect(corners);
    #endregion 普通匹配

    #region Knn匹配

    # KnnMatch 方法，接受查询图像和可选参数，使用默认的描述符匹配器类型（FlannBased）
    public Point2f KnnMatch(Mat queryMat, Mat? queryMatMask = null,
        DescriptorMatcherType matcherType = DescriptorMatcherType.FlannBased)
    {
        # 调用重载的 KnnMatch 方法
        return KnnMatch(_trainKeyPoints, _trainDescriptors, queryMat, queryMatMask, matcherType);
    }

    # KnnMatch 方法，接受查询图像和前一个点的坐标，返回匹配的点
    public Point2f KnnMatch(Mat queryMat, float prevX, float prevY, Mat? queryMatMask = null,
        DescriptorMatcherType matcherType = DescriptorMatcherType.FlannBased)
    {
        # 根据前一个点的坐标计算所在的特征块的行列索引
        var (cellRow, cellCol) = KeyPointFeatureBlockHelper.GetCellIndex(_trainMatSize, _splitRow, _splitCol, prevX, prevY);
        # 输出当前坐标所在的特征块信息到调试窗口
        Debug.WriteLine($"当前坐标({prevX},{prevY})在特征块({cellRow},{cellCol})中");
        # 如果上一个合并的特征块为空或不在当前特征块中，则更新合并的特征块
        if (_lastMergedBlock == null || _lastMergedBlock.MergedCenterCellRow != cellRow || _lastMergedBlock.MergedCenterCellCol != cellCol)
        {
            # 输出切换到新特征块的调试信息
            Debug.WriteLine($"---------切换到新的特征块({cellRow},{cellCol})，合并特征点--------");
            # 合并当前特征块的邻近特征点
            _lastMergedBlock = KeyPointFeatureBlockHelper.MergeNeighboringFeatures(_blocks, _trainDescriptors, cellRow, cellCol);
        }

        # 使用合并后的特征点调用 KnnMatch 方法
        return KnnMatch(_lastMergedBlock.KeyPointArray, _lastMergedBlock.Descriptor!, queryMat, queryMatMask, matcherType);
    }

    /// <summary>
    /// https://github.com/tignioj/minimap/blob/main/matchmap/sifttest/sifttest5.py
    /// Copilot 生成
    /// </summary>
    /// <returns></returns>
    # KnnMatch 方法，接受训练数据和查询图像，返回匹配的点
    private Point2f KnnMatch(KeyPoint[] trainKeyPoints, Mat trainDescriptors, Mat queryMat, Mat? queryMatMask = null,
        DescriptorMatcherType matcherType = DescriptorMatcherType.FlannBased)
    {
        # 创建 SpeedTimer 对象，用于测量执行时间
        SpeedTimer speedTimer = new();
        # 使用查询图像创建 Mat 对象用于存储查询图像的描述符
        using var queryDescriptors = new Mat();
#pragma warning disable CS8604 // 禁用警告，指示可能存在引用类型参数为 null 的问题
        _feature2D.DetectAndCompute(queryMat, queryMatMask, out var queryKeyPoints, queryDescriptors);
        # 进行特征点检测和描述符计算，结果存储在 queryKeyPoints 和 queryDescriptors 中
#pragma warning restore CS8604 // 恢复之前禁用的警告
        speedTimer.Record("模板生成KeyPoint");
        # 记录时间点，标记模板生成关键点操作的时间
        var matches = GetMatcher(matcherType).KnnMatch(queryDescriptors, trainDescriptors, k: 2);
        # 使用 k-最近邻算法匹配查询描述符和训练描述符，k=2 表示每个查询描述符寻找两个最佳匹配
        speedTimer.Record("FlannMatch");
        # 记录时间点，标记 FLANN 匹配操作的时间

        // 应用比例测试来过滤匹配点
        List<DMatch> goodMatches = [];
        # 初始化存储好的匹配点的列表
        foreach (var match in matches)
        # 遍历所有的匹配点
        {
            if (match.Length == 2 && match[0].Distance < 0.75 * match[1].Distance)
            # 如果匹配点的数量是 2 且第一个匹配点的距离小于第二个匹配点距离的 0.75 倍
            {
                goodMatches.Add(match[0]);
                # 将第一个匹配点添加到好的匹配点列表中
            }
        }

        if (goodMatches.Count < 7)
        # 如果好的匹配点数量少于 7 个
        {
            return new Point2f();
            # 返回一个空的 Point2f 对象
        }

        // 获取匹配点的坐标
        var srcPts = goodMatches.Select(m => queryKeyPoints[m.QueryIdx].Pt).ToArray();
        # 从好的匹配点中提取查询图像的关键点坐标
        var dstPts = goodMatches.Select(m => trainKeyPoints[m.TrainIdx].Pt).ToArray();
        # 从好的匹配点中提取训练图像的关键点坐标

        speedTimer.Record("GetGoodMatchPoints");
        # 记录时间点，标记提取好的匹配点坐标操作的时间

        // 使用RANSAC找到变换矩阵
        var mask = new Mat();
        # 创建一个用于存储 RANSAC 结果掩码的 Mat 对象
        var hMat = Cv2.FindHomography(srcPts.ToList().ToPoint2d(), dstPts.ToList().ToPoint2d(), HomographyMethods.Ransac, 3.0, mask);
        # 使用 RANSAC 方法计算源点和目标点之间的单应矩阵
        if (hMat.Empty())
        # 如果计算得到的单应矩阵为空
        {
            return new Point2f();
            # 返回一个空的 Point2f 对象
        }
        speedTimer.Record("FindHomography");
        # 记录时间点，标记计算单应矩阵操作的时间

        // 计算小地图的中心点
        var h = queryMat.Rows;
        # 获取查询图像的行数（高度）
        var w = queryMat.Cols;
        # 获取查询图像的列数（宽度）
        var centerPoint = new Point2f(w / 2f, h / 2f);
        # 计算查询图像的中心点
        Point2f[] centerPoints = [centerPoint];
        # 创建包含中心点的数组
        Point2f[] transformedCenter = Cv2.PerspectiveTransform(centerPoints, hMat);
        # 使用计算得到的单应矩阵将中心点转换到目标图像的坐标系中

        speedTimer.Record("PerspectiveTransform");
        # 记录时间点，标记透视变换操作的时间
        speedTimer.DebugPrint();
        # 打印时间调试信息
        // 返回小地图在大地图中的中心坐标
        return transformedCenter[0];
        # 返回转换后的中心点坐标
    }

    public Point2f[] KnnMatchCorners(KeyPoint[] trainKeyPoints, Mat trainDescriptors, Mat queryMat, Mat? queryMatMask = null,
        DescriptorMatcherType matcherType = DescriptorMatcherType.FlannBased)
    {
        SpeedTimer speedTimer = new();
        # 创建一个 SpeedTimer 实例用于记录时间
        using var queryDescriptors = new Mat();
        # 使用 using 声明来确保 queryDescriptors 在使用完后能够正确释放资源
#pragma warning disable CS8604 // 禁用警告，指示可能存在引用类型参数为 null 的问题
        _feature2D.DetectAndCompute(queryMat, queryMatMask, out var queryKeyPoints, queryDescriptors);
        # 进行特征点检测和描述符计算，结果存储在 queryKeyPoints 和 queryDescriptors 中
#pragma warning restore CS8604 // 恢复对可能为 null 的引用类型参数的警告
        // 记录“模板生成KeyPoint”步骤的时间
        speedTimer.Record("模板生成KeyPoint");
        // 使用匹配器进行 K 最近邻匹配，找到每个查询描述符的两个最接近的训练描述符
        var matches = GetMatcher(matcherType).KnnMatch(queryDescriptors, trainDescriptors, k: 2);
        // 记录“FlannMatch”步骤的时间
        speedTimer.Record("FlannMatch");

        // 应用比例测试来过滤匹配点
        List<DMatch> goodMatches = []; // 初始化用于存储过滤后匹配点的列表
        foreach (var match in matches) // 遍历所有匹配点
        {
            // 仅保留符合比例测试的匹配点
            if (match.Length == 2 && match[0].Distance < 0.75 * match[1].Distance)
            {
                goodMatches.Add(match[0]); // 添加符合条件的匹配点
            }
        }

        // 如果匹配点不足 7 个，返回空列表
        if (goodMatches.Count < 7)
        {
            return [];
        }

        // 获取匹配点的坐标
        var srcPts = goodMatches.Select(m => queryKeyPoints[m.QueryIdx].Pt).ToArray(); // 查询图像的点坐标
        var dstPts = goodMatches.Select(m => trainKeyPoints[m.TrainIdx].Pt).ToArray(); // 训练图像的点坐标

        // 记录“GetGoodMatchPoints”步骤的时间
        speedTimer.Record("GetGoodMatchPoints");

        // 使用 RANSAC 方法找到变换矩阵
        var mask = new Mat(); // 初始化遮罩矩阵
        var hMat = Cv2.FindHomography(srcPts.ToList().ToPoint2d(), dstPts.ToList().ToPoint2d(), HomographyMethods.Ransac, 3.0, mask);
        // 如果变换矩阵为空，返回空列表
        if (hMat.Empty())
        {
            return [];
        }
        // 记录“FindHomography”步骤的时间
        speedTimer.Record("FindHomography");

        // 定义四个角点的坐标
        var objCorners = new Point2f[4]; // 初始化角点数组
        objCorners[0] = new Point2f(0, 0); // 左上角
        objCorners[1] = new Point2f(0, queryMat.Rows); // 左下角
        objCorners[2] = new Point2f(queryMat.Cols, queryMat.Rows); // 右下角
        objCorners[3] = new Point2f(queryMat.Cols, 0); // 右上角

        // 使用透视变换矩阵转换角点坐标
        var sceneCorners = Cv2.PerspectiveTransform(objCorners, hMat);
        // 记录“PerspectiveTransform”步骤的时间
        speedTimer.Record("PerspectiveTransform");
        // 输出调试信息
        speedTimer.DebugPrint();
        // 返回变换后的角点坐标
        return sceneCorners;
    }

    public Rect KnnMatchRect(Mat queryMat, Mat? queryMatMask = null)
    {
        // 获取 K 最近邻匹配的角点
        var corners = KnnMatchCorners(_trainKeyPoints, _trainDescriptors, queryMat, queryMatMask);
        // 如果没有角点，返回空矩形
        if (corners.Length == 0)
        {
            return Rect.Empty;
        }
        // 返回包含所有角点的最小矩形
        return Cv2.BoundingRect(corners);
    }

    #endregion Knn匹配
```