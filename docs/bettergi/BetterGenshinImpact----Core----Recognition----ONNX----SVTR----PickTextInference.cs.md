# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\ONNX\SVTR\PickTextInference.cs`

```cs
﻿using BetterGenshinImpact.Core.Config;
using Microsoft.ML.OnnxRuntime;
using Microsoft.ML.OnnxRuntime.Tensors;
using OpenCvSharp;
using System;
using System.Buffers;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using System.Text;
using System.Text.Json;

namespace BetterGenshinImpact.Core.Recognition.ONNX.SVTR;

/// <summary>
///     来自于 Yap 的拾取文字识别
///     https://github.com/Alex-Beng/Yap
/// </summary>
public class PickTextInference : ITextInference
{
    private readonly InferenceSession _session; // 保存 ONNX 模型的推理会话
    private readonly Dictionary<int, string> _wordDictionary; // 字典，将模型输出的索引映射到实际的单词

    public PickTextInference()
    {
        // 获取模型文件的绝对路径
        var modelPath = Global.Absolute("Assets\\Model\\Yap\\model_training.onnx");
        // 检查模型文件是否存在，不存在则抛出异常
        if (!File.Exists(modelPath)) throw new FileNotFoundException("Yap模型文件不存在", modelPath);

        // 创建 ONNX 模型的推理会话
        _session = new InferenceSession(modelPath, BgiSessionOption.Instance.Options);

        // 获取字典文件的绝对路径
        var wordJsonPath = Global.Absolute("Assets\\Model\\Yap\\index_2_word.json");
        // 检查字典文件是否存在，不存在则抛出异常
        if (!File.Exists(wordJsonPath)) throw new FileNotFoundException("Yap字典文件不存在", wordJsonPath);

        // 读取字典文件内容
        var json = File.ReadAllText(wordJsonPath);
        // 将 JSON 内容解析为字典，无法解析则抛出异常
        _wordDictionary = JsonSerializer.Deserialize<Dictionary<int, string>>(json) ?? throw new Exception("index_2_word.json deserialize failed");
    }

    public string Inference(Mat mat)
    {
        // 记录开始时间
        long startTime = Stopwatch.GetTimestamp();
        // 将输入的 Mat 对象转换为 (1, 1, 32, 384) 形状的张量
        var reshapedInputData = ToTensorUnsafe(mat, out var owner);

        IDisposableReadOnlyCollection<DisposableNamedOnnxValue> results;

        using (owner)
        {
            // 创建 NamedOnnxValue 输入，并运行模型推理
            results = _session.Run([NamedOnnxValue.CreateFromTensor("input", reshapedInputData)]);
        }

        using (results)
        {
            // 获取模型输出的张量数据
            var boxes = results[0].AsTensor<float>();

            var ans = new StringBuilder(); // 用于构建识别结果字符串
            var lastWord = default(string); // 上一个词，用于避免重复识别
            for (var i = 0; i < boxes.Dimensions[0]; i++)
            {
                var maxIndex = 0; // 保存当前最大值的索引
                var maxValue = -1.0; // 当前最大值
                for (var j = 0; j < _wordDictionary.Count; j++)
                {
                    // 获取当前索引处的值
                    var value = boxes[[i, 0, j]];
                    // 更新最大值和最大值索引
                    if (value > maxValue)
                    {
                        maxValue = value;
                        maxIndex = j;
                    }
                }

                // 获取最大值索引对应的单词
                var word = _wordDictionary[maxIndex];
                // 如果当前单词与上一个单词不同且不为分隔符，则添加到结果中
                if (word != lastWord && word != "|")
                {
                    ans.Append(word);
                }

                // 更新上一个单词
                lastWord = word;
            }

            // 计算模型推理时间
            TimeSpan time = Stopwatch.GetElapsedTime(startTime);
            // 将结果转换为字符串
            string result = ans.ToString();
            // 输出识别时间和结果
            Debug.WriteLine($"Yap模型识别 耗时{time.TotalMilliseconds}ms 结果: {result}");
            // 返回识别结果
            return result;
        }
    }

    public static Tensor<float> ToTensorUnsafe(Mat src, out IMemoryOwner<float> tensorMemoryOwnser)
    {
        // 获取源图像的通道数
        var channels = src.Channels();
        // 获取源图像的行数
        var nRows = src.Rows;
        // 计算源图像的总列数（列数乘以通道数）
        var nCols = src.Cols * channels;
        // 如果源图像是连续的（数据在内存中是一块连续的区域）
        if (src.IsContinuous())
        {
            // 计算源图像的总列数（行数乘以列数）
            nCols *= nRows;
            // 设置行数为1，因为数据在内存中是连续的一维数组
            nRows = 1;
        }

        // 分配一块内存来存储转换后的浮点数据
        tensorMemoryOwnser = MemoryPool<float>.Shared.Rent(nCols);
        // 创建内存的视图，只包含前 nCols 个元素
        var memory = tensorMemoryOwnser.Memory[..nCols];
        unsafe
        {
            // 遍历每一行
            for (var i = 0; i < nRows; i++)
            {
                // 获取当前行的字节指针
                var b = (byte*)src.Ptr(i);
                // 遍历每一列
                for (var j = 0; j < nCols; j++)
                {
                    // 将每个字节值归一化到 0 到 1 之间，并存储到内存视图中
                    memory.Span[j] = b[j] / 255f;
                }
            }
        }

        // 创建一个新的 DenseTensor 对象，形状为 [1, 1, 32, 384]，并返回
        return new DenseTensor<float>(memory, [1, 1, 32, 384]);
    }
# 这是一个花括号，通常用于表示代码块的结束
}
```