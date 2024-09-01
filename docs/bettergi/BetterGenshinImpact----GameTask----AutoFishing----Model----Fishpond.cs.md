# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFishing\Model\Fishpond.cs`

```cs
# 引用相关命名空间和库
﻿using BetterGenshinImpact.Core.Recognition.OpenCv;
using Compunet.YoloV8.Data;
using OpenCvSharp;
using System;
using System.Collections.Generic;
using System.Linq;

namespace BetterGenshinImpact.GameTask.AutoFishing.Model;

public class Fishpond
{
    /// <summary>
    /// 鱼池位置
    /// </summary>
    public Rect FishpondRect { get; set; }

    /// <summary>
    /// 抛竿落点位置
    /// </summary>
    public Rect TargetRect { get; set; }

    /// <summary>
    /// 鱼池中的鱼
    /// </summary>
    public List<OneFish> Fishes { get; set; } = [];

    # 构造函数，初始化 Fishpond 对象
    public Fishpond(DetectionResult result)
    {
        # 遍历检测结果中的每个边界框
        foreach (var box in result.Boxes)
        {
            # 如果边界框属于 "rod" 类别，设置 TargetRect
            if (box.Class.Name == "rod")
            {
                TargetRect = new Rect(box.Bounds.X, box.Bounds.Y, box.Bounds.Width, box.Bounds.Height);
                continue;
            }
            # 如果边界框属于 "err rod" 类别，设置 TargetRect
            else if (box.Class.Name == "err rod")
            {
                TargetRect = new Rect(box.Bounds.X, box.Bounds.Y, box.Bounds.Width, box.Bounds.Height);
                continue;
            }

            # 创建 OneFish 对象并添加到 Fishes 列表
            var fish = new OneFish(box.Class.Name, new Rect(box.Bounds.X, box.Bounds.Y, box.Bounds.Width, box.Bounds.Height), box.Confidence);
            Fishes.Add(fish);
        }

        # 按鱼的可信度降序排列 Fishes 列表
        Fishes = [.. Fishes.OrderByDescending(fish => fish.Confidence)];

        # 计算并设置鱼池的位置
        FishpondRect = CalculateFishpondRect();
    }

    /// <summary>
    /// 计算鱼塘位置
    /// </summary>
    /// <returns></returns>
    public Rect CalculateFishpondRect()
    {
        # 如果没有鱼，则返回空矩形
        if (Fishes.Count == 0)
        {
            return Rect.Empty;
        }

        # 初始化鱼池的边界
        var left = int.MaxValue;
        var top = int.MaxValue;
        var right = int.MinValue;
        var bottom = int.MinValue;
        # 遍历每条鱼，更新鱼池的边界
        foreach (var fish in Fishes)
        {
            if (fish.Rect.Left < left)
            {
                left = fish.Rect.Left;
            }

            if (fish.Rect.Top < top)
            {
                top = fish.Rect.Top;
            }

            if (fish.Rect.Right > right)
            {
                right = fish.Rect.Right;
            }

            if (fish.Rect.Bottom > bottom)
            {
                bottom = fish.Rect.Bottom;
            }
        }

        # 返回计算得到的鱼池矩形
        return new Rect(left, top, right - left, bottom - top);
    }

    /// <summary>
    /// 通过鱼饵名称过滤鱼
    /// </summary>
    /// <param name="baitName"></param>
    /// <returns></returns>
    public List<OneFish> FilterByBaitName(string baitName)
    {
        # 按鱼饵名称过滤 Fishes 列表，并按可信度降序排列
        return [.. Fishes.Where(fish => fish.FishType.BaitName == baitName).OrderByDescending(fish => fish.Confidence)];
    }

    public OneFish? FilterByBaitNameAndRecently(string baitName, Rect prevTargetFishRect)
    # 通过指定的饵名称过滤鱼类
    var fishes = FilterByBaitName(baitName);
    # 如果没有找到鱼类，返回 null
    if (fishes.Count == 0)
    {
        return null;
    }

    # 初始化最小距离为最大可能值
    var min = double.MaxValue;
    # 获取之前目标鱼的中心点
    var c1 = prevTargetFishRect.GetCenterPoint();
    # 初始化结果变量为 null
    OneFish? result = null;
    # 遍历所有找到的鱼
    foreach (var fish in fishes)
    {
        # 获取当前鱼的中心点
        var c2 = fish.Rect.GetCenterPoint();
        # 计算之前目标鱼中心点到当前鱼中心点的距离
        var distance = Math.Sqrt(Math.Pow(c1.X - c2.X, 2) + Math.Pow(c1.Y - c2.Y, 2));
        # 如果计算得到的距离小于已知最小距离，则更新最小距离和结果鱼
        if (distance < min)
        {
            min = distance;
            result = fish;
        }
    }

    # 返回距离最小的鱼
    return result;
    # <summary>
    # 返回吃最多鱼饵的名称
    # </summary>
    # <returns></returns>
    public string MostMatchBait()
    {
        # 创建一个字典用于记录每种鱼饵的数量
        Dictionary<string, int> dict = [];
        # 遍历所有鱼
        foreach (var fish in Fishes)
        {
            # 如果字典中已经存在当前鱼的饵名称，则数量加 1
            if (dict.TryGetValue(fish.FishType.BaitName, out _))
            {
                dict[fish.FishType.BaitName]++;
            }
            # 否则，将饵名称添加到字典中，数量初始化为 1
            else
            {
                dict[fish.FishType.BaitName] = 1;
            }
        }

        # 初始化最大数量为 0
        var max = 0;
        # 初始化结果变量为空字符串
        var result = "";
        # 遍历字典中的所有饵名称及其数量
        foreach (var (key, value) in dict)
        {
            # 如果当前饵的数量大于已知最大数量，则更新最大数量和结果饵名称
            if (value > max)
            {
                max = value;
                result = key;
            }
        }

        # 返回数量最多的饵名称
        return result;
    }
# 结束当前代码块或语句的闭合
}
```