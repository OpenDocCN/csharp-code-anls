# `.\better-genshin-impact\BetterGenshinImpact.Test\Dataset\AvatarClassifyGen.cs`

```cs
﻿using System.Diagnostics; // 导入系统诊断命名空间，用于调试功能
using System.IO; // 导入系统输入输出命名空间，用于文件操作
using OpenCvSharp; // 导入 OpenCVSharp 库，用于计算机视觉处理

namespace BetterGenshinImpact.Test.Dataset; // 定义命名空间，用于组织代码

public class AvatarClassifyGen
{
    // 基础图像文件夹路径常量
    private const string BaseDir = @"E:\HuiTask\更好的原神\自动秘境\自动战斗\队伍识别\分类器\";

    // 背景图像文件夹路径，结合基础图像文件夹路径和 "background" 文件夹
    private static readonly string BackgroundDir = Path.Combine(BaseDir, "background");

    // 随机数生成器
    private static readonly Random Rd = new Random();

    public static void GenAll()
    {
        // 读取基础图像文件夹中的指定图像文件
        // List<string> sideImageFiles = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "*.png", SearchOption.TopDirectoryOnly).ToList();
        // 只用一个图像文件
        List<string> sideImageFiles = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "UI_AvatarIcon_Side_Emilie.png", SearchOption.TopDirectoryOnly).ToList();
        // 读取另一图像文件并添加到列表中
        List<string> sideImageFiles2 = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "UI_AvatarIcon_Side_MomokaCostumeErrantry.png", SearchOption.TopDirectoryOnly).ToList();
        sideImageFiles.AddRange(sideImageFiles2);
        // 读取另一图像文件并添加到列表中
        List<string> sideImageFiles3 = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "UI_AvatarIcon_Side_NilouCostumeFairy.png", SearchOption.TopDirectoryOnly).ToList();
        sideImageFiles.AddRange(sideImageFiles3);
        // 读取另一图像文件并添加到列表中
        List<string> sideImageFiles4 = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "UI_AvatarIcon_Side_Kachina.png", SearchOption.TopDirectoryOnly).ToList();
        sideImageFiles.AddRange(sideImageFiles4);
        // 读取另一图像文件并添加到列表中
        List<string> sideImageFiles5 = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "UI_AvatarIcon_Side_Mualani.png", SearchOption.TopDirectoryOnly).ToList();
        sideImageFiles.AddRange(sideImageFiles5);
        // 读取另一图像文件并添加到列表中
        List<string> sideImageFiles6 = Directory.GetFiles(Path.Combine(BaseDir, "side_src"), "UI_AvatarIcon_Side_Kinich.png", SearchOption.TopDirectoryOnly).ToList();
        sideImageFiles.AddRange(sideImageFiles6);

        // 生成训练集数据
        GenTo(sideImageFiles, Path.Combine(BaseDir, @"dateset\train"), 200);
        // 生成测试集数据
        GenTo(sideImageFiles, Path.Combine(BaseDir, @"dateset\test"), 40);
        // GenTo(new List<string> { sideImageFiles[1] }, Path.Combine(BaseDir, @"dateset\test"), 1); // 注释掉的代码，用于生成单个测试集数据
    }

    static void GenTo(List<string> sideImageFiles, string dataFolder, int count)
    {
        // 方法体尚未实现
    }
}
```