# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\Element\Assets\MapAssets.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间
using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.GameTask.AutoTrackPath.Model 命名空间
using BetterGenshinImpact.GameTask.AutoTrackPath.Model;
# 引入 BetterGenshinImpact.GameTask.Model 命名空间
using BetterGenshinImpact.GameTask.Model;
# 引入 BetterGenshinImpact.Service 命名空间
using BetterGenshinImpact.Service;
# 引入 OpenCvSharp 命名空间
using OpenCvSharp;
# 引入 System 命名空间
using System;
# 引入 System.Collections.Generic 命名空间
using System.Collections.Generic;
# 引入 System.IO 命名空间
using System.IO;
# 引入 System.Text.Json 命名空间
using System.Text.Json;

# 定义命名空间 BetterGenshinImpact.GameTask.Common.Element.Assets
namespace BetterGenshinImpact.GameTask.Common.Element.Assets;

# 创建 MapAssets 类，继承自 BaseAssets<MapAssets>
public class MapAssets : BaseAssets<MapAssets>
{
    # 定义 MainMap2048BlockMat 属性，惰性加载 Mat 对象，图片路径为指定路径，模式为灰度
    public Lazy<Mat> MainMap2048BlockMat { get; } = new(() => new Mat(@"E:\HuiTask\更好的原神\地图匹配\有用的素材\5.0\mainMap2048Block.png", ImreadModes.Grayscale));

    # 定义 MainMap256BlockMat 属性，惰性加载 Mat 对象，图片路径为 Global.Absolute 方法提供的路径，模式为灰度
    public Lazy<Mat> MainMap256BlockMat { get; } = new(() => new Mat(Global.Absolute(@"Assets\Map\mainMap256Block.png"), ImreadModes.Grayscale));

    # 2048 区块下，存在传送点的最大面积，识别结果比这个大的话，需要点击放大

    # 传送点信息
    public List<GiWorldPosition> TpPositions;

    # 每个地区点击后处于的中心位置，使用字典来存储每个地区的坐标
    public readonly Dictionary<string, double[]> CountryPositions = new()
    {
        # 定义“蒙德”地区的中心位置坐标
        { "蒙德", [-876, 2278] },
        # 定义“璃月”地区的中心位置坐标
        { "璃月", [270, -666] },
        # 定义“稻妻”地区的中心位置坐标
        { "稻妻", [-4400, -3050] },
        # 定义“须弥”地区的中心位置坐标
        { "须弥", [2877, -374] },
        # 定义“枫丹”地区的中心位置坐标
        { "枫丹", [4515, 3631] },
    };

    # 构造函数
    public MapAssets()
    {
        # 读取 JSON 文件内容到字符串
        var json = File.ReadAllText(Global.Absolute(@"GameTask\AutoTrackPath\Assets\tp.json"));
        # 反序列化 JSON 字符串为 List<GiWorldPosition> 对象，如果失败则抛出异常
        TpPositions = JsonSerializer.Deserialize<List<GiWorldPosition>>(json, ConfigService.JsonOptions) ?? throw new Exception("tp.json deserialize failed");
    }
}
```