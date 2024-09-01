# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\ONNX\SVTR\ITextInference.cs`

```cs
# 引入 OpenCvSharp 库，用于处理计算机视觉任务
﻿using OpenCvSharp;

# 定义命名空间，组织代码结构
namespace BetterGenshinImpact.Core.Recognition.ONNX.SVTR
{
    # 定义接口，描述文字识别推理功能的契约
    /// <summary>
    ///     文字识别推理(SVTR网络)
    /// </summary>
    public interface ITextInference
    {
        # 定义接口方法，接受 OpenCV 的 Mat 对象，返回识别结果的字符串
        public string Inference(Mat mat);
    }
}
```