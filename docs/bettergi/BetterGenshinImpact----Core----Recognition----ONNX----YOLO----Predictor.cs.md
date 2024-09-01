# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\ONNX\YOLO\Predictor.cs`

```cs
# 引用 BetterGenshinImpact.Core.Config 命名空间
using BetterGenshinImpact.Core.Config;
# 引用 Microsoft.ML.OnnxRuntime 命名空间
using Microsoft.ML.OnnxRuntime;
# 引用 System 命名空间
using System;
# 引用 System.IO 命名空间
using System.IO;
# 引用 System.Text.Json 命名空间
using System.Text.Json;

# 定义命名空间 BetterGenshinImpact.Core.Recognition.ONNX.YOLO
namespace BetterGenshinImpact.Core.Recognition.ONNX.YOLO;

# 表示此类已过时，未来可能会被删除
[Obsolete]
# 定义 Predictor 类
public class Predictor
{
    # 定义私有字段 _session，类型为 InferenceSession
    private readonly InferenceSession _session;
    # 定义私有字段 labels，类型为字符串数组
    private readonly string[] labels;

    # Predictor 类的构造函数
    public Predictor()
    {
        # 创建一个新的 SessionOptions 实例
        var options = new SessionOptions();
        # 获取模型文件的绝对路径
        var modelPath = Global.Absolute("Assets\\Model\\Fish\\bgi_fish.onnx");
        # 如果模型文件不存在，则抛出 FileNotFoundException
        if (!File.Exists(modelPath)) throw new FileNotFoundException("自动钓鱼模型文件不存在", modelPath);

        # 使用模型路径和选项创建 InferenceSession 实例
        _session = new InferenceSession(modelPath, options);

        # 获取标签文件的绝对路径
        var wordJsonPath = Global.Absolute("Assets\\Model\\Fish\\label.json");
        # 如果标签文件不存在，则抛出 FileNotFoundException
        if (!File.Exists(wordJsonPath)) throw new FileNotFoundException("自动钓鱼模型分类文件不存在", wordJsonPath);

        # 读取标签文件的内容
        var json = File.ReadAllText(wordJsonPath);
        # 反序列化 JSON 内容为字符串数组，并赋值给 labels，如果失败则抛出异常
        labels = JsonSerializer.Deserialize<string[]>(json) ?? throw new Exception("label.json deserialize failed");
    }
}
```