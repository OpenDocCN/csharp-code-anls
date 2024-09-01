# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\ExpeditionTask.cs`

```cs
﻿using BetterGenshinImpact.Core.Recognition.OCR; // 引入 OCR 识别相关库
using BetterGenshinImpact.GameTask.AutoSkip.Model; // 引入自动跳过任务模型相关库
using BetterGenshinImpact.GameTask.Common; // 引入公共任务功能库
using BetterGenshinImpact.View.Drawable; // 引入视图可绘制对象库
using Microsoft.Extensions.Logging; // 引入日志记录功能库
using OpenCvSharp; // 引入 OpenCV 库用于计算机视觉
using Sdcb.PaddleOCR; // 引入 PaddleOCR 库用于 OCR 处理
using System; // 引入系统基本功能库
using System.Collections.Generic; // 引入集合功能库
using System.Drawing; // 引入图形功能库
using System.Linq; // 引入 LINQ 查询功能库

namespace BetterGenshinImpact.GameTask.AutoSkip; // 定义命名空间用于自动跳过任务

/// <summary>
/// 重新探索派遣
///
/// 必须在已经有探索派遣完成的情况下使用
///
/// 于 4.3 版本废弃
/// </summary>
[Obsolete] // 标记类已废弃
public class ExpeditionTask
{
    private static readonly List<string> ExpeditionCharacterList = []; // 定义静态列表存储探索角色

    private int _expeditionCount = 0; // 定义私有字段用于记录派遣次数

    public void Run(CaptureContent content)
    {
        InitConfig(); // 初始化配置
        var assetScale = TaskContext.Instance().SystemInfo.AssetScale; // 获取资源缩放比例
        ReExplorationGameArea(content); // 重新探索游戏区域
        for (var i = 0; i <= 4; i++) // 遍历派遣位置
        {
            if (_expeditionCount >= 5) // 如果派遣次数达到最大值
            {
                // 最多派遣5人
                break; // 退出循环
            }
            else
            {
                content.CaptureRectArea
                    .Derive(new Rect((int)(110 * assetScale), (int)((145 + 70 * i) * assetScale),
                        (int)(60 * assetScale), (int)(33 * assetScale)))
                    .Click(); // 点击派遣区域
                TaskControl.Sleep(500); // 暂停500毫秒
                ReExplorationGameArea(content); // 重新探索游戏区域
            }
        }

        TaskControl.Logger.LogInformation("探索派遣：{Text}", "重新派遣完成"); // 记录信息日志
        VisionContext.Instance().DrawContent.ClearAll(); // 清除所有绘制内容
    }

    private void InitConfig()
    {
        var str = TaskContext.Instance().Config.AutoSkipConfig.AutoReExploreCharacter; // 获取自动重新探索角色配置
        if (!string.IsNullOrEmpty(str)) // 如果配置字符串不为空
        {
            ExpeditionCharacterList.Clear(); // 清空探索角色列表
            str = str.Replace("，", ","); // 替换中文逗号为英文逗号
            str.Split(',').ToList().ForEach(x => ExpeditionCharacterList.Add(x.Trim())); // 将配置字符串拆分为角色列表
            TaskContext.Instance().Config.AutoSkipConfig.AutoReExploreCharacter = string.Join(",", ExpeditionCharacterList); // 更新配置字符串
        }
    }

    private void ReExplorationGameArea(CaptureContent content) // 定义重新探索游戏区域的方法
    # 获取捕捉区域的矩形
    var captureRect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
    # 获取资产缩放比例
    var assetScale = TaskContext.Instance().SystemInfo.AssetScale;

    # 循环5次，检查和处理任务
    for (var i = 0; i < 5; i++)
    {
        # 捕捉区域并进行 OCR 识别，查找 "探险完成" 文本
        var result = CaptureAndOcr(content, new Rect(0, 0, captureRect.Width - (int)(480 * assetScale), captureRect.Height));
        var rect = result.FindRectByText("探险完成");
        # TODO i>1 时通过“探索派遣限制 4 / 5 ”判断是否完成派遣
        if (rect != Rect.Empty)
        {
            # 点击 "探险完成" 下方的头像区域
            content.CaptureRectArea.Derive(new Rect(rect.X, rect.Y + (int)(50 * assetScale), rect.Width, (int)(80 * assetScale))).Click();
            TaskControl.Sleep(100);
            # 重新捕捉并查找 "领取" 文本
            result = CaptureAndOcr(content);
            rect = result.FindRectByText("领取");
            if (rect != Rect.Empty)
            {
                using var ra = content.CaptureRectArea.Derive(rect);
                ra.Click();
                #TaskControl.Logger.LogInformation("探索派遣：点击{Text}", "领取");
                TaskControl.Sleep(200);
                # 点击空白区域以继续
                ra.Click();
                TaskControl.Sleep(250);

                # 选择角色
                result = CaptureAndOcr(content);
                rect = result.FindRectByText("选择角色");
                if (rect != Rect.Empty)
                {
                    content.CaptureRectArea.Derive(rect).Click();
                    TaskControl.Sleep(400); # 等待动画完成
                    var success = SelectCharacter(content);
                    if (success)
                    {
                        _expeditionCount++;
                    }
                }
            }
            else
            {
                TaskControl.Logger.LogWarning("探索派遣：找不到 {Text} 文字", "领取");
            }
        }
        else
        {
            # 如果没有找到 "探险完成" 文本，则退出循环
            break;
        }
    }

    # 选择角色的私有方法
    private bool SelectCharacter(CaptureContent content)
    {
        # 获取捕捉区域的矩形
        var captureRect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        # 捕捉区域的一半并进行 OCR 识别
        var result = CaptureAndOcr(content, new Rect(0, 0, captureRect.Width / 2, captureRect.Height));
        if (result.RegionHasText("角色选择"))
        {
            # 获取所有角色卡片
            var cards = GetCharacterCards(result);
            if (cards.Count > 0)
            {
                # 选择空闲且名称在 ExpeditionCharacterList 中的角色，若无则选择第一个空闲角色
                var card = cards.FirstOrDefault(c => c.Idle && c.Name != null && ExpeditionCharacterList.Contains(c.Name)) ?? cards.First(c => c.Idle);
                var rect = card.Rects.First();

                using var ra = content.CaptureRectArea.Derive(rect);
                ra.Click();
                TaskControl.Logger.LogInformation("探索派遣：派遣 {Name}", card.Name);
                TaskControl.Sleep(500);
                return true;
            }
        }

        return false;
    }

    /// <summary>
    # 根据文字识别结果获取所有角色选项
    /// </summary>
    /// <param name="result"></param>
    /// <returns></returns>
    private List<ExpeditionCharacterCard> GetCharacterCards(PaddleOcrResult result)
    {
        // 获取捕获区域的矩形
        var captureRect = TaskContext.Instance().SystemInfo.CaptureAreaRect;
        // 获取资源的缩放比例
        var assetScale = TaskContext.Instance().SystemInfo.AssetScale;

        // 将 OCR 结果的区域转换为 OcrResultRect 对象，筛选出位于捕获区域左半部分的区域，并按 Y 坐标和 X 坐标排序
        var ocrResultRects = result.Regions
            .Select(x => x.ToOcrResultRect())
            .Where(r => r.Rect.X + r.Rect.Width < captureRect.Width / 2)
            .OrderBy(r => r.Rect.Y)
            .ThenBy(r => r.Rect.X)
            .ToList();

        // 初始化存储角色卡片的列表
        var cards = new List<ExpeditionCharacterCard>();
        // 遍历 OCR 结果区域
        foreach (var ocrResultRect in ocrResultRects)
        {
            // 如果区域的文本包含指定的关键字，则创建新的角色卡片
            if (ocrResultRect.Text.Contains("时间缩短") || ocrResultRect.Text.Contains("奖励增加") || ocrResultRect.Text.Contains("暂无加成"))
            {
                // 创建新的角色卡片对象
                var card = new ExpeditionCharacterCard();
                // 将当前区域的矩形添加到角色卡片中
                card.Rects.Add(ocrResultRect.Rect);
                // 将区域的文本内容作为附加信息存储到角色卡片中
                card.Addition = ocrResultRect.Text;
                // 遍历 OCR 结果区域，查找与当前区域相关的区域
                foreach (var ocrResultRect2 in ocrResultRects)
                {
                    // 检查当前区域是否在指定的 Y 坐标范围内
                    if (ocrResultRect2.Rect.Y > ocrResultRect.Rect.Y - 50 * assetScale
                        && ocrResultRect2.Rect.Y + ocrResultRect2.Rect.Height < ocrResultRect.Rect.Y + ocrResultRect.Rect.Height)
                    {
                        // 如果区域的文本包含“探险完成”或“探险中”，则设置角色卡片的状态和名称
                        if (ocrResultRect2.Text.Contains("探险完成") || ocrResultRect2.Text.Contains("探险中"))
                        {
                            card.Idle = false;
                            var name = ocrResultRect2.Text.Replace("探险完成", "").Replace("探险中", "").Replace("/", "").Trim();
                            if (!string.IsNullOrEmpty(name))
                            {
                                card.Name = name;
                            }
                        }
                        // 如果文本不包含上述关键字，则将其作为角色名称
                        else if (!ocrResultRect2.Text.Contains("时间缩短") && !ocrResultRect2.Text.Contains("奖励增加") && !ocrResultRect2.Text.Contains("暂无加成"))
                        {
                            card.Name = ocrResultRect2.Text;
                        }

                        // 将相关区域的矩形添加到角色卡片中
                        card.Rects.Add(ocrResultRect2.Rect);
                    }
                }

                // 如果角色卡片的名称不为空，则将其添加到角色卡片列表中
                if (!string.IsNullOrEmpty(card.Name))
                {
                    cards.Add(card);
                }
                // 否则，记录警告日志
                else
                {
                    TaskControl.Logger.LogWarning("探索派遣：存在未找到角色命的识别内容");
                }
            }
        }

        // 返回角色卡片列表
        return cards;
    }

    // 创建一个红色的画笔，宽度为 1
    private readonly Pen _pen = new(Color.Red, 1);

    private PaddleOcrResult CaptureAndOcr(CaptureContent content)
    {
        // 捕获指定内容的矩形区域并获取灰度图像
        using var ra = TaskControl.CaptureToRectArea();
        // 使用 OCR 工厂对灰度图像进行 OCR 处理，返回 OCR 结果
        var result = OcrFactory.Paddle.OcrResult(ra.SrcGreyMat);
        //VisionContext.Instance().DrawContent.PutOrRemoveRectList("OcrResultRects", result.ToRectDrawableList(_pen));
        // 返回 OCR 结果
        return result;
    }

    private PaddleOcrResult CaptureAndOcr(CaptureContent content, Rect rect)
    # 使用 TaskControl 类的方法捕获矩形区域并自动释放资源
    using var ra = TaskControl.CaptureToRectArea();
    # 使用 Paddle OCR 工厂从捕获的灰度图像中获取 OCR 结果
    var result = OcrFactory.Paddle.OcrResult(ra.SrcGreyMat);
    # 注释掉的代码行：将 OCR 结果绘制到 VisionContext 的内容中，带有矩形列表
    //VisionContext.Instance().DrawContent.PutOrRemoveRectList("OcrResultRects", result.ToRectDrawableList(_pen));
    # 返回 OCR 结果
    return result;
你提供的代码只有一个
```