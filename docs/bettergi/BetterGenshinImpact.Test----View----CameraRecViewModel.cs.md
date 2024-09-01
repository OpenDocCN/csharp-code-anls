# `.\better-genshin-impact\BetterGenshinImpact.Test\View\CameraRecViewModel.cs`

```cs
# 导入相关命名空间
﻿using BetterGenshinImpact.Test.Simple.MiniMap;
using OxyPlot;
using OxyPlot.Series;

namespace BetterGenshinImpact.Test.View;

# 定义一个内部类 CameraRecViewModel
internal class CameraRecViewModel
{
    # 定义三个只读属性，分别用于存储三个绘图模型
    public PlotModel LeftModel { get; private set; }
    public PlotModel RightModel { get; private set; }
    public PlotModel AllModel { get; private set; }

    # CameraRecViewModel 类的构造函数
    public CameraRecViewModel()
    {
        # 调用 CameraOrientationTest.Test1() 方法获取数据
        var data = CameraOrientationTest.Test1();

        # 使用数据创建绘图模型，并分别赋值给 LeftModel、RightModel 和 AllModel 属性
        LeftModel = BuildModel(data.Item1, "左");
        RightModel = BuildModel(data.Item2, "右(左移90度后)");
        AllModel = BuildModel(data.Item3, "乘积");
    }

    # 创建绘图模型的方法
    public PlotModel BuildModel(int[] data, string name)
    {
        # 创建折线图系列
        var series = new LineSeries();
        # 将数据点添加到折线图系列中
        for (int i = 0; i < data.Length; i++)
        {
            series.Points.Add(new DataPoint(i, data[i]));
        }

        # 创建绘图模型，并将折线图系列添加到模型中
        var plotModel = new PlotModel();
        plotModel.Series.Add(series);

        # 设置绘图模型的标题
        plotModel.Title = name;
        return plotModel;
    }
}
```