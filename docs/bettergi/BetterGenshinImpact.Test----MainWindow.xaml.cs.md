# `.\better-genshin-impact\BetterGenshinImpact.Test\MainWindow.xaml.cs`

```cs
# 引入所需的命名空间
﻿using System.Windows;
using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.GameTask.Common.Map;
using BetterGenshinImpact.Test.Dataset;
using BetterGenshinImpact.Test.Simple;
using BetterGenshinImpact.Test.Simple.AllMap;
using BetterGenshinImpact.Test.Simple.Track;
using BetterGenshinImpact.Test.View;

# 定义命名空间
namespace BetterGenshinImpact.Test;

# MainWindow.xaml 的交互逻辑
public partial class MainWindow : Window
{
    # 构造函数，初始化窗口并设置全局启动路径
    public MainWindow()
    {
        InitializeComponent();
        # 设置全局启动路径
        Global.StartUpPath = @"D:\HuiPrograming\Projects\CSharp\MiHoYo\BetterGenshinImpact\BetterGenshinImpact\bin\x64\Debug\net8.0-windows10.0.22621.0";
    }

    # 显示摄像机录制窗口
    private void ShowCameraRecWindow(object sender, System.Windows.RoutedEventArgs e)
    {
        new CameraRecWindow().Show();
    }

    # 显示 HSV 测试窗口并运行
    private void ShowHsvTestWindow(object sender, System.Windows.RoutedEventArgs e)
    {
        new HsvTestWindow().Run();
    }

    # 执行地图拼图操作
    private void DoMapPuzzle(object sender, System.Windows.RoutedEventArgs e)
    {
        MapPuzzle.Put();
    }

    # 执行 OCR 测试
    private void DoOcrTest(object sender, System.Windows.RoutedEventArgs e)
    {
        OcrTest.TestYap();
    }

    # 执行模板匹配测试
    private void DoMatchTemplateTest(object sender, System.Windows.RoutedEventArgs e)
    {
        MatchTemplateTest.Test();
    }

    # 执行匹配测试，包括存储整个地图测试
    private void DoMatchTest(object sender, System.Windows.RoutedEventArgs e)
    {
        // KeyPointMatchTest.Test();
        // EntireMapTest.Test();
        EntireMapTest.Storage();
        // BigMapMatchTest.Test();

        // FeatureTransfer.Transfer();
    }

    # 绘制地图传送点
    private void MapDrawTeleportPoint(object sender, RoutedEventArgs e)
    {
        MapTeleportPointDraw.Draw();
    }

    # 生成头像数据
    private void GenAvatarData(object sender, RoutedEventArgs e)
    {
        AvatarClassifyGen.GenAll();
    }

    # 自动烹饪测试用例
    private void AutoCookTestCase(object sender, RoutedEventArgs e)
    {
        AutoCookTest.Test();
    }

    # 显示地图路径
    private void MapPathView(object sender, RoutedEventArgs e)
    {
        MapPathTest.Test();
    }

    # 放大测试
    private void ZoomOut(object sender, RoutedEventArgs e)
    {
        ScaleTest.ZoomOutTest();
    }
}
```