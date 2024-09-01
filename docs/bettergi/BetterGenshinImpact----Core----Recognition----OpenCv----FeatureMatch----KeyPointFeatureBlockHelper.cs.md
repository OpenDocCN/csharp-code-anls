# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\FeatureMatch\KeyPointFeatureBlockHelper.cs`

```cs
using BetterGenshinImpact.Core.Recognition.OpenCv.Model; // 引入自定义模型定义的命名空间
using OpenCvSharp; // 引入 OpenCVSharp 库的命名空间
using System; // 引入基本系统功能的命名空间
using System.Collections.Generic; // 引入集合类的命名空间
using System.Runtime.CompilerServices; // 引入编译器相关功能的命名空间
using System.Runtime.InteropServices; // 引入与互操作相关的功能的命名空间

namespace BetterGenshinImpact.Core.Recognition.OpenCv.FeatureMatch; // 定义一个命名空间

public class KeyPointFeatureBlockHelper // 定义一个公共类
{
    public static KeyPointFeatureBlock[][] SplitFeatures(Size originalImage, int rows, int cols, KeyPoint[] keyPoints, Mat matches)
    {
        var matchesCols = matches.Cols; // 获取匹配矩阵的列数，SURF 64，SIFT 128
        // Calculate grid size
        int cellWidth = originalImage.Width / cols; // 计算每个网格单元的宽度
        int cellHeight = originalImage.Height / rows; // 计算每个网格单元的高度

        // Initialize arrays to store split features and matches
        var splitKeyPoints = new KeyPointFeatureBlock[rows][]; // 创建一个二维数组用于存储分割的特征块

        // Initialize each row
        for (int i = 0; i < rows; i++)
        {
            splitKeyPoints[i] = new KeyPointFeatureBlock[cols]; // 为每一行初始化特征块数组
        }

        // Split features and matches
        for (int i = 0; i < keyPoints.Length; i++)
        {
            int row = (int)(keyPoints[i].Pt.Y / cellHeight); // 根据 Y 坐标确定网格的行
            int col = (int)(keyPoints[i].Pt.X / cellWidth); // 根据 X 坐标确定网格的列

            // Ensure row and col are within bounds
            row = Math.Min(Math.Max(row, 0), rows - 1); // 确保行索引在有效范围内
            col = Math.Min(Math.Max(col, 0), cols - 1); // 确保列索引在有效范围内

            // ReSharper disable once ConditionIsAlwaysTrueOrFalseAccordingToNullableAPIContract
            if (splitKeyPoints[row][col] == null) // 如果该网格单元尚未初始化
            {
                splitKeyPoints[row][col] = new KeyPointFeatureBlock(); // 初始化该网格单元
            }

            splitKeyPoints[row][col].KeyPointList.Add(keyPoints[i]); // 将关键点添加到对应网格单元的列表中
            splitKeyPoints[row][col].KeyPointIndexList.Add(i); // 将关键点的索引添加到对应网格单元的索引列表中
        }

        // 遍历每个特征块，计算描述子
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
            {
                // ReSharper disable once ConditionIsAlwaysTrueOrFalseAccordingToNullableAPIContract
                if (splitKeyPoints[i][j] == null) // 如果该网格单元为空
                {
                    continue; // 跳过该网格单元
                }

                var block = splitKeyPoints[i][j]; // 获取当前网格单元的特征块

                var descriptor = new Mat(block.KeyPointIndexList.Count, matchesCols, MatType.CV_32FC1); // 创建一个描述子矩阵
                InitBlockMat(block.KeyPointIndexList, descriptor, matches); // 初始化描述子矩阵

                block.Descriptor = descriptor; // 将描述子矩阵赋给特征块
            }
        }

        return splitKeyPoints; // 返回分割后的特征块数组
    }

    public static (int, int) GetCellIndex(Size originalImage, int rows, int cols, float x, float y)
    {
        // Calculate grid size
        int cellWidth = originalImage.Width / cols; // 计算每个网格单元的宽度
        int cellHeight = originalImage.Height / rows; // 计算每个网格单元的高度

        // Calculate cell index for the given point
        var cellRow = (int)Math.Round(y / cellHeight, 0); // 根据 Y 坐标计算网格的行索引
        var cellCol = (int)Math.Round(x / cellWidth, 0); // 根据 X 坐标计算网格的列索引

        return (cellRow, cellCol); // 返回计算得到的网格行列索引
    }

    public static KeyPointFeatureBlock MergeNeighboringFeatures(KeyPointFeatureBlock[][] splitKeyPoints, Mat matches, int cellRow, int cellCol)
    {
        // 获取匹配的列数，SURF 64  SIFT 128
        var matchesCols = matches.Cols; // SURF 64  SIFT 128
        // 初始化用于存储邻近特征和匹配的列表
        var neighboringKeyPoints = new List<KeyPoint>();
        var neighboringKeyPointIndices = new List<int>();

        // 遍历 9 个邻近单元格
        for (int i = Math.Max(cellRow - 1, 0); i <= Math.Min(cellRow + 1, splitKeyPoints.Length - 1); i++)
        {
            for (int j = Math.Max(cellCol - 1, 0); j <= Math.Min(cellCol + 1, splitKeyPoints[i].Length - 1); j++)
            {
                // 检查当前单元格是否为空
                // ReSharper disable once ConditionIsAlwaysTrueOrFalseAccordingToNullableAPIContract
                if (splitKeyPoints[i][j] != null)
                {
                    // 将当前单元格中的关键点和其索引添加到列表中
                    neighboringKeyPoints.AddRange(splitKeyPoints[i][j].KeyPointList);
                    neighboringKeyPointIndices.AddRange(splitKeyPoints[i][j].KeyPointIndexList);
                }
            }
        }

        // 合并邻近特征
        var mergedKeyPointBlock = new KeyPointFeatureBlock
        {
            // 设置合并后的中心单元格列和行
            MergedCenterCellCol = cellCol,
            MergedCenterCellRow = cellRow,
            // 设置合并后的关键点列表和索引列表
            KeyPointList = neighboringKeyPoints,
            KeyPointIndexList = neighboringKeyPointIndices
        };
        // 初始化描述符矩阵，行数为关键点索引列表的长度，列数为匹配的列数
        mergedKeyPointBlock.Descriptor = new Mat(mergedKeyPointBlock.KeyPointIndexList.Count, matchesCols, MatType.CV_32FC1);
        // 调用方法初始化描述符矩阵
        InitBlockMat(mergedKeyPointBlock.KeyPointIndexList, mergedKeyPointBlock.Descriptor, matches);

        // 返回合并后的关键点块
        return mergedKeyPointBlock;
    }

    /// <summary>
    /// 按行拷贝matches到descriptor
    /// </summary>
    /// <param name="keyPointIndexList">记录了哪些行需要拷贝</param>
    /// <param name="descriptor">拷贝结果</param>
    /// <param name="matches">原始数据</param>
    private static unsafe void InitBlockMat(List<int> keyPointIndexList, Mat descriptor, Mat matches)
    {
        // 获取原始数据的尺寸
        Size size = matches.Size();

        // 创建源矩阵和目标矩阵
        Matrix<float> destMatrix = new(descriptor.DataPointer, size.Width, size.Height);
        Matrix<float> sourceMatrix = new(matches.DataPointer, size.Width, size.Height);

        // 将关键点索引列表转换为 Span 类型
        Span<int> keyPointIndexSpan = CollectionsMarshal.AsSpan(keyPointIndexList);
        ref int keyPointIndexRef = ref MemoryMarshal.GetReference(keyPointIndexSpan);
        // 遍历关键点索引并将源矩阵的数据拷贝到目标矩阵
        for (int i = 0; i < keyPointIndexSpan.Length; i++)
        {
            ref int index = ref Unsafe.Add(ref keyPointIndexRef, i);
            sourceMatrix[index].CopyTo(destMatrix[i]);
        }
    }

    // 只读的矩阵结构体
    private readonly ref struct Matrix<T>
    {
        // 定义一个只读引用字段，指向类型 T 的数据
        private readonly ref T reference;
        // 定义矩阵的宽度
        private readonly int width;
        // 定义矩阵的高度
        private readonly int height;

        // 构造函数，接受指针、宽度和高度，初始化矩阵
        public unsafe Matrix(void* pointer, int width, int height)
        {
            // 使用指针创建类型 T 的引用
            reference = ref Unsafe.AsRef<T>(pointer);
            // 初始化宽度
            this.width = width;
            // 初始化高度
            this.height = height;
        }

        // 索引器，根据行号获取对应的 Span
        public Span<T> this[int row]
        {
            get
            {
                // 检查行号是否在有效范围内
                if (row < 0 || row >= height)
                {
                    // 如果无效，抛出索引超出范围异常
                    throw new IndexOutOfRangeException();
                }

                // 返回行的 Span，通过计算偏移量来创建
                return MemoryMarshal.CreateSpan(ref Unsafe.Add(ref reference, row * width), width);
            }
        }
    }

    /// <summary>
    /// 按行拷贝matches到descriptor
    /// </summary>
    /// <param name="keyPointIndexList">记录了哪些行需要拷贝</param>
    /// <param name="descriptor">拷贝结果</param>
    /// <param name="matches">原始数据</param>
    [Obsolete]
    private static unsafe void InitBlockMat2(IReadOnlyList<int> keyPointIndexList, Mat descriptor, Mat matches)
    {
        // 获取 matches 的列数
        int cols = matches.Cols;
        // 获取 descriptor 的数据指针，转换为 float 指针
        float* ptrDest = (float*)descriptor.DataPointer;

        // 遍历每个需要拷贝的行
        for (int i = 0; i < keyPointIndexList.Count; i++)
        {
            // 获取当前行的索引
            var index = keyPointIndexList[i];
            // 获取 matches 中对应行的指针
            float* ptrSrcRow = (float*)matches.Ptr(index);
            // 拷贝每列的数据到 descriptor
            for (int j = 0; j < cols; j++)
            {
                *(ptrDest + i * cols + j) = ptrSrcRow[j];
            }
        }
    }
# 代码块结束的右大括号，表示代码的结束
}
```