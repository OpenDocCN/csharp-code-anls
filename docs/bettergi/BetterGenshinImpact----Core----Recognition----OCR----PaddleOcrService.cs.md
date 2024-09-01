# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OCR\PaddleOcrService.cs`

```cs
﻿using BetterGenshinImpact.Core.Config; // 引入配置相关的命名空间
using OpenCvSharp; // 引入 OpenCVSharp 库，用于图像处理
using Sdcb.PaddleInference; // 引入 PaddleInference 库，用于推理
using Sdcb.PaddleOCR; // 引入 PaddleOCR 库，用于 OCR 识别
using Sdcb.PaddleOCR.Models; // 引入 PaddleOCR 模型相关类
using System; // 引入系统命名空间
using System.Diagnostics; // 引入调试相关的命名空间
using System.IO; // 引入文件输入输出的命名空间

namespace BetterGenshinImpact.Core.Recognition.OCR; // 定义命名空间

public class PaddleOcrService : IOcrService // 定义 PaddleOcrService 类实现 IOcrService 接口
{
    private static readonly object locker = new(); // 定义静态对象用于线程安全

    /// <summary>
    ///     Usage:
    ///     https://github.com/sdcb/PaddleSharp/blob/master/docs/ocr.md
    ///     模型列表:
    ///     https://github.com/PaddlePaddle/PaddleOCR/blob/release/2.5/doc/doc_ch/models_list.md
    /// </summary>
    private readonly PaddleOcrAll _paddleOcrAll; // 定义私有只读字段存储 PaddleOCR 实例

    public PaddleOcrService() // 构造函数
    {
        var path = Global.Absolute("Assets\\Model\\PaddleOcr"); // 获取模型文件所在的绝对路径
        var localDetModel = DetectionModel.FromDirectory(Path.Combine(path, "ch_PP-OCRv4_det"), ModelVersion.V4); // 从指定目录加载检测模型
        var localClsModel = ClassificationModel.FromDirectory(Path.Combine(path, "ch_ppocr_mobile_v2.0_cls")); // 从指定目录加载分类模型
        var localRecModel = RecognizationModel.FromDirectory(Path.Combine(path, "ch_PP-OCRv4_rec"), Path.Combine(path, "ppocr_keys_v1.txt"), ModelVersion.V4); // 从指定目录加载识别模型
        var model = new FullOcrModel(localDetModel, localClsModel, localRecModel); // 创建完整的 OCR 模型
        // Action<PaddleConfig> device = TaskContext.Instance().Config.InferenceDevice switch
        // {
        //     "CPU" => PaddleDevice.Onnx(), // 使用 CPU 推理设备
        //     "GPU_DirectML" => PaddleDevice.Onnx(), // 使用 GPU 推理设备
        //     _ => throw new InvalidEnumArgumentException("无效的推理设备") // 无效的推理设备类型
        // };
        _paddleOcrAll = new PaddleOcrAll(model, PaddleDevice.Onnx()) // 初始化 PaddleOcrAll 实例
        {
            AllowRotateDetection = false, /* 允许识别有角度的文字 */
            Enable180Classification = false /* 允许识别旋转角度大于90度的文字 */
        };

        // System.AccessViolationException
        // https://github.com/babalae/better-genshin-impact/releases/latest
        // 下载并解压到相同目录下
    }

    public string Ocr(Mat mat) // 执行 OCR 并返回识别结果的文本
    {
        return OcrResult(mat).Text; // 调用 OcrResult 方法获取结果并返回文本
    }

    public PaddleOcrResult OcrResult(Mat mat) // 执行 OCR 并返回完整的识别结果
    {
        lock (locker) // 锁定以确保线程安全
        {
            long startTime = Stopwatch.GetTimestamp(); // 记录开始时间
            var result = _paddleOcrAll.Run(mat); // 执行 OCR 识别
            TimeSpan time = Stopwatch.GetElapsedTime(startTime); // 计算耗时
            Debug.WriteLine($"PaddleOcr 耗时 {time.TotalMilliseconds}ms 结果: {result.Text}"); // 输出调试信息
            return result; // 返回 OCR 结果
        }
    }

    public string OcrWithoutDetector(Mat mat) // 执行 OCR 识别但不使用检测器
    {
        lock (locker) // 锁定以确保线程安全
        {
            var str = _paddleOcrAll.Recognizer.Run(mat).Text; // 执行 OCR 识别并获取文本
            Debug.WriteLine($"PaddleOcrWithoutDetector 结果: {str}"); // 输出调试信息
            return str; // 返回识别结果
        }
    }
}
```