# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\PathPointRecorder.cs`

```cs
# 引入必要的命名空间和库
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Core.Recognition.OpenCv;
using BetterGenshinImpact.GameTask.AutoGeniusInvokation.Exception;
using BetterGenshinImpact.GameTask.AutoTrackPath.Model;
using BetterGenshinImpact.GameTask.Common.Element.Assets;
using BetterGenshinImpact.GameTask.Common.Map;
using BetterGenshinImpact.GameTask.Model.Enum;
using BetterGenshinImpact.Helpers.Extensions;
using BetterGenshinImpact.Model;
using BetterGenshinImpact.Service;
using Microsoft.Extensions.Logging;
using OpenCvSharp;
using System;
using System.Diagnostics;
using System.IO;
using System.Text.Json;
using System.Threading;
using System.Threading.Tasks;
using static BetterGenshinImpact.GameTask.Common.TaskControl;

namespace BetterGenshinImpact.GameTask.AutoTrackPath;

# 定义 PathPointRecorder 类，继承自 Singleton 模式
public class PathPointRecorder : Singleton<PathPointRecorder>
{
    # 初始化一个表示整个地图的私有字段
    private readonly EntireMap _bigMap = EntireMap.Instance;

    # 用于记录任务的私有字段
    private Task? _recordTask;
    # 用于取消记录任务的私有字段
    private CancellationTokenSource? _recordTaskCts;

    # 切换记录状态的方法
    public void Switch()
    {
        try
        {
            # 如果记录任务为空，则启动新任务
            if (_recordTask == null)
            {
                # 设置任务触发器为只缓存模式
                TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.OnlyCacheCapture);

                # 创建取消令牌源并启动记录任务
                _recordTaskCts = new CancellationTokenSource();
                _recordTask = RecordTask(_recordTaskCts);
                _recordTask.Start();
            }
            else
            {
                # 如果记录任务已存在，则恢复正常触发模式
                TaskTriggerDispatcher.Instance().SetCacheCaptureMode(DispatcherCaptureModeEnum.NormalTrigger);

                # 取消当前记录任务并将任务字段置空
                _recordTaskCts?.Cancel();
                _recordTask = null;
            }
        }
        # 捕捉正常结束异常
        catch (NormalEndException)
        {
            Logger.LogInformation("关闭路线录制");
        }
        # 捕捉其他异常并输出
        catch (Exception e)
        {
            Console.WriteLine(e);
        }
    }

    # 声明一个记录任务的方法，接受取消令牌源作为参数
    public Task RecordTask(CancellationTokenSource cts)
    {
        // 创建一个新的任务，用于异步执行指定的操作
        return new Task(() =>
        {
            // 初始化 GiPath 对象
            GiPath way = new();

            // 当取消请求未被触发时，持续执行循环
            while (!cts.Token.IsCancellationRequested)
            {
                try
                {
                    // 休眠 10 毫秒，并检查取消请求
                    Sleep(10, cts);
                    // 捕捉当前矩形区域的图像
                    var ra = CaptureToRectArea();

                    // 小地图匹配
                    // 获取模板图像
                    var tar = ElementAssets.Instance.PaimonMenuRo.TemplateImageGreyMat!;
                    // 在捕捉到的图像区域中进行模板匹配
                    var p = MatchTemplateHelper.MatchTemplate(ra.SrcGreyMat, tar, TemplateMatchModes.CCoeffNormed, null, 0.9);
                    // 如果匹配结果无效，休眠 50 毫秒并继续循环
                    if (p.X == 0 || p.Y == 0)
                    {
                        Sleep(50, cts);
                        continue;
                    }

                    // 根据匹配结果获取大地图中小地图的位置
                    var p2 = _bigMap.GetMiniMapPositionByFeatureMatch(new Mat(ra.SrcGreyMat, new Rect(p.X + 24, p.Y - 15, 210, 210)));
                    // 如果位置有效，则添加到路径中并输出调试信息
                    if (!p2.IsEmpty())
                    {
                        way.AddPoint(p2);
                        Debug.WriteLine($"AddPoint: {p2}");
                    }
                    else
                    {
                        // 如果位置无效，休眠 50 毫秒
                        Sleep(50, cts);
                    }
                }
                catch (Exception e)
                {
                    // 输出异常信息
                    Console.WriteLine(e);
                }
            }
#if DEBUG
            # 如果处于调试模式，则将序列化后的数据写入到日志文件中
            File.WriteAllText(Global.Absolute($@"log\way\{DateTime.Now:yyyy-MM-dd HH：mm：ss：ffff}.json"), JsonSerializer.Serialize(way, ConfigService.JsonOptions));
#endif
            # 记录信息，表示路线录制已经结束
            Logger.LogInformation("路线录制结束");
        }, cts.Token);
    }
}
```