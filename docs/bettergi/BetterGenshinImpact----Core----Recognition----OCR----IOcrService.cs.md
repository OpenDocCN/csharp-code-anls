# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OCR\IOcrService.cs`

```cs
# 使用 OpenCvSharp 和 Sdcb.PaddleOCR 库进行 OCR 相关操作
using OpenCvSharp;  # 引入 OpenCvSharp 库，用于图像处理
using Sdcb.PaddleOCR;  # 引入 Sdcb.PaddleOCR 库，用于 OCR 识别

namespace BetterGenshinImpact.Core.Recognition.OCR;  # 定义命名空间，用于 OCR 相关功能

# 定义一个接口 IOcrService，声明 OCR 服务的公共方法
public interface IOcrService
{
    # 定义一个方法，接受 Mat 类型的图像，返回识别结果的字符串
    public string Ocr(Mat mat);

    # 定义一个方法，接受 Mat 类型的图像，返回未经过检测器处理的识别结果的字符串
    public string OcrWithoutDetector(Mat mat);

    # 定义一个方法，接受 Mat 类型的图像，返回 PaddleOcrResult 类型的 OCR 结果
    public PaddleOcrResult OcrResult(Mat mat);
}
```