# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoTrackPath\TpTask.cs`

```cs
# 引入 BetterGenshinImpact.Core.Recognition 命名空间
using BetterGenshinImpact.Core.Recognition;
# 引入 BetterGenshinImpact.Core.Recognition.OpenCv 命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;
# 引入 BetterGenshinImpact.Core.Simulator 命名空间
using BetterGenshinImpact.Core.Simulator;
# 引入 BetterGenshinImpact.GameTask.Common.BgiVision 命名空间
using BetterGenshinImpact.GameTask.Common.BgiVision;
# 引入 BetterGenshinImpact.GameTask.Common.Element.Assets 命名空间
using BetterGenshinImpact.GameTask.Common.Element.Assets;
# 引入 BetterGenshinImpact.GameTask.Common.Map 命名空间
using BetterGenshinImpact.GameTask.Common.Map;
# 引入 BetterGenshinImpact.GameTask.Model.Area 命名空间
using BetterGenshinImpact.GameTask.Model.Area;
# 引入 BetterGenshinImpact.GameTask.QuickTeleport.Assets 命名空间
using BetterGenshinImpact.GameTask.QuickTeleport.Assets;
# 引入 BetterGenshinImpact.Helpers.Extensions 命名空间
using BetterGenshinImpact.Helpers.Extensions;
# 引入 Microsoft.Extensions.Logging 命名空间
using Microsoft.Extensions.Logging;
# 引入 OpenCvSharp 命名空间
using OpenCvSharp;
# 引入 System 命名空间
using System;
# 引入 System.Diagnostics 命名空间
using System.Diagnostics;
# 引入 System.Linq 命名空间
using System.Linq;
# 引入 System.Threading 命名空间
using System.Threading;
# 引入 System.Threading.Tasks 命名空间
using System.Threading.Tasks;
# 引入 Vanara.PInvoke 命名空间
using Vanara.PInvoke;
# 静态导入 BetterGenshinImpact.GameTask.Common.TaskControl 命名空间中的所有成员
using static BetterGenshinImpact.GameTask.Common.TaskControl;

# 定义 BetterGenshinImpact.GameTask.AutoTrackPath 命名空间
namespace BetterGenshinImpact.GameTask.AutoTrackPath
{
    # 定义 TpTask 类，接收一个 CancellationTokenSource 参数
    public class TpTask(CancellationTokenSource cts)
    {
        # 定义只读字段 _assets，初始化为 QuickTeleportAssets 的单例实例
        private readonly QuickTeleportAssets _assets = QuickTeleportAssets.Instance;

        # 定义公共异步方法 Tp，接受目标传送坐标 tpX 和 tpY
        /// <summary>
        /// 通过大地图传送到指定坐标最近的传送点，然后移动到指定坐标
        /// </summary>
        /// <param name="tpX"></param>
        /// <param name="tpY"></param>
        public async Task Tp(double tpX, double tpY)
    {
        // 获取最近的传送点位置
        var (x, y) = GetRecentlyTpPoint(tpX, tpY); 
        // 记录传送点的坐标信息
        Logger.LogInformation("({TpX},{TpY}) 最近的传送点位置 ({X},{Y})", tpX, tpY, x, y);

        // M 打开地图识别当前位置，中心点为当前位置
        using var ra1 = CaptureToRectArea(); 
        // 如果当前不在大地图界面，模拟按下 M 键，切换到大地图界面
        if (!Bv.IsInBigMapUi(ra1))
        {
            Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_M);
            // 延迟一秒以等待界面切换完成
            await Delay(1000, cts);
        }

        // 切换到包含传送点的地图
        await SwitchRecentlyCountryMap(x, y);

        // 计算传送点位置是否在当前大地图范围内，如果不在则继续移动地图
        var bigMapInAllMapRect = GetBigMapRect(); 
        while (!bigMapInAllMapRect.Contains(x, y))
        {
            // 记录传送点不在当前大地图范围内的信息
            Debug.WriteLine($"({x},{y}) 不在 {bigMapInAllMapRect} 内，继续移动");
            Logger.LogInformation("传送点不在当前大地图范围内，继续移动");
            // 移动地图到传送点位置
            await MoveMapTo(x, y);
            // 更新大地图范围
            bigMapInAllMapRect = GetBigMapRect();
        }

        // Debug.WriteLine($"({x},{y}) 在 {bigMapInAllMapRect} 内，计算它在窗体内的位置");
        // 将传送点坐标从游戏坐标系转换为主窗体坐标系
        var (picX, picY) = MapCoordinate.GameToMain2048(x, y); 
        var picRect = MapCoordinate.GameToMain2048(bigMapInAllMapRect); 
        // 记录计算后的坐标信息
        Debug.WriteLine($"({picX},{picY}) 在 {picRect} 内，计算它在窗体内的位置");
        // 获取系统信息中的捕获区域
        var captureRect = TaskContext.Instance().SystemInfo.ScaleMax1080PCaptureRect; 
        // 计算点击坐标相对于捕获区域的坐标
        var clickX = (int)((picX - picRect.X) / picRect.Width * captureRect.Width); 
        var clickY = (int)((picY - picRect.Y) / picRect.Height * captureRect.Height); 
        // 记录计算得到的点击坐标信息
        Logger.LogInformation("点击传送点：({X},{Y})", clickX, clickY);
        // 捕获当前区域并点击传送点位置
        using var ra = CaptureToRectArea(); 
        ra.ClickTo(clickX, clickY); 

        // 触发一次快速传送功能
        await Delay(500, cts); 
        await ClickTpPoint(CaptureToRectArea()); 

        // 等待传送完成，通过检查是否回到主界面
        for (var i = 0; i < 20; i++)
        {
            await Delay(1200, cts); 
            using var ra3 = CaptureToRectArea(); 
            // 如果检测到在主界面中则退出循环
            if (Bv.IsInMainUi(ra3))
            {
                break; 
            }
        }

        // 记录传送完成的信息
        Logger.LogInformation("传送完成"); 
    }

    /// <summary>
    /// 移动地图到指定传送点位置
    /// 可能会移动不对，所以可以重试此方法
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    public async Task MoveMapTo(double x, double y) 
    {
        // 获取大地图的中心点位置
        var bigMapCenterPoint = GetPositionFromBigMap();
        // 计算当前位置与大地图中心点的偏移量
        var (xOffset, yOffset) = (x - bigMapCenterPoint.X, y - bigMapCenterPoint.Y);

        var diffMouseX = 100; // 每次移动鼠标的距离
        if (xOffset < 0)
        {
            // 如果 xOffset 小于 0，反转 diffMouseX 的方向
            diffMouseX = -diffMouseX;
        }

        var diffMouseY = 100; // 每次移动鼠标的距离
        if (yOffset < 0)
        {
            // 如果 yOffset 小于 0，反转 diffMouseY 的方向
            diffMouseY = -diffMouseY;
        }

        // 移动到屏幕中心附近的随机位置，避免地图移动无效
        await MouseMoveMapX(diffMouseX);
        await MouseMoveMapY(diffMouseY);
        // 获取移动后的新地图中心点位置
        var newBigMapCenterPoint = GetPositionFromBigMap();
        // 计算地图移动的实际距离
        var diffMapX = Math.Abs(newBigMapCenterPoint.X - bigMapCenterPoint.X);
        var diffMapY = Math.Abs(newBigMapCenterPoint.Y - bigMapCenterPoint.Y);
        Debug.WriteLine($"每100移动的地图距离：({diffMapX},{diffMapY})");

        // 如果地图移动距离大于 10，则快速移动到目标区域
        if (diffMapX > 10 && diffMapY > 10)
        {
            // 计算需要移动的次数
            var moveCount = (int)Math.Abs(xOffset / diffMapX); // 向下取整
            Debug.WriteLine("X需要移动的次数：" + moveCount);
            for (var i = 0; i < moveCount; i++)
            {
                // 按照计算次数移动 X 方向
                await MouseMoveMapX(diffMouseX);
            }

            moveCount = (int)Math.Abs(yOffset / diffMapY); // 向下取整
            Debug.WriteLine("Y需要移动的次数：" + moveCount);
            for (var i = 0; i < moveCount; i++)
            {
                // 按照计算次数移动 Y 方向
                await MouseMoveMapY(diffMouseY);
            }
        }
    }

    public async Task MouseMoveMapX(int dx)
    {
        // 根据 dx 确定移动单元的大小
        var moveUnit = dx > 0 ? 20 : -20;
        // 在游戏区域内随机移动以避免地图移动无效
        GameCaptureRegion.GameRegionMove((rect, _) => (rect.Width / 2d + Random.Shared.Next(-rect.Width / 6, rect.Width / 6), rect.Height / 2d + Random.Shared.Next(-rect.Height / 6, rect.Height / 6)));
        // 按下鼠标左键以开始移动
        Simulation.SendInput.Mouse.LeftButtonDown();
        await Delay(200, cts); // 等待 200 毫秒
        for (var i = 0; i < dx / moveUnit; i++)
        {
            // 按 moveUnit 移动鼠标，60 毫秒的延迟避免惯性
            Simulation.SendInput.Mouse.MoveMouseBy(moveUnit, 0).Sleep(60);
        }

        // 释放鼠标左键
        Simulation.SendInput.Mouse.LeftButtonUp();
        await Delay(200, cts); // 等待 200 毫秒
    }

    public async Task MouseMoveMapY(int dy)
    {
        // 根据 dy 确定移动单元的大小
        var moveUnit = dy > 0 ? 20 : -20;
        // 在游戏区域内随机移动以避免地图移动无效
        GameCaptureRegion.GameRegionMove((rect, _) => (rect.Width / 2d + Random.Shared.Next(-rect.Width / 6, rect.Width / 6), rect.Height / 2d + Random.Shared.Next(-rect.Height / 6, rect.Height / 6)));
        // 按下鼠标左键以开始移动
        Simulation.SendInput.Mouse.LeftButtonDown();
        await Delay(200, cts); // 等待 200 毫秒
        // 原神地图在小范围内移动是无效的，所以先随便移动一下，确保有效移动
        for (var i = 0; i < dy / moveUnit; i++)
        {
            // 按 moveUnit 移动鼠标，60 毫秒的延迟避免惯性
            Simulation.SendInput.Mouse.MoveMouseBy(0, moveUnit).Sleep(60);
        }

        // 释放鼠标左键
        Simulation.SendInput.Mouse.LeftButtonUp();
        await Delay(200, cts); // 等待 200 毫秒
    }

    public Point2f GetPositionFromBigMap()
    {
        // 获取大地图的中心点位置
        return GetBigMapCenterPoint();
    }

    public Rect GetBigMapRect()
    {
        // 判断是否在地图界面
        using var ra = CaptureToRectArea(); // 创建一个用于捕捉区域的对象
        using var mapScaleButtonRa = ra.Find(QuickTeleportAssets.Instance.MapScaleButtonRo); // 在捕捉区域中查找地图缩放按钮
        if (mapScaleButtonRa.IsExist()) // 如果找到地图缩放按钮
        {
            // 根据捕捉区域的灰度图，获取大地图的位置矩形
            var rect = BigMap.Instance.GetBigMapRectByFeatureMatch(ra.SrcGreyMat);
            if (rect == Rect.Empty) // 如果未能识别大地图位置
            {
                throw new InvalidOperationException("识别大地图位置失败"); // 抛出异常
            }

            // 输出识别到的大地图位置矩形
            Debug.WriteLine("识别大地图在全地图位置矩形：" + rect);
            const int s = BigMap.ScaleTo2048; // 相对1024做4倍缩放
            // 将大地图的位置矩形按比例转换为游戏坐标系中的位置
            return MapCoordinate.Main2048ToGame(new Rect(rect.X * s, rect.Y * s, rect.Width * s, rect.Height * s));
        }
        else
        {
            throw new InvalidOperationException("当前不在地图界面"); // 如果没有找到地图缩放按钮，抛出异常
        }
    }

    public Point2f GetBigMapCenterPoint()
    {
        // 判断是否在地图界面
        using var ra = CaptureToRectArea(); // 创建一个用于捕捉区域的对象
        using var mapScaleButtonRa = ra.Find(QuickTeleportAssets.Instance.MapScaleButtonRo); // 在捕捉区域中查找地图缩放按钮
        if (mapScaleButtonRa.IsExist()) // 如果找到地图缩放按钮
        {
            // 根据捕捉区域的灰度图，获取大地图的位置
            var p = BigMap.Instance.GetBigMapPositionByFeatureMatch(ra.SrcGreyMat);
            if (p.IsEmpty()) // 如果未能识别大地图位置
            {
                throw new InvalidOperationException("识别大地图位置失败"); // 抛出异常
            }

            // 输出识别到的大地图位置
            Debug.WriteLine("识别大地图在全地图位置：" + p);
            // 将大地图位置按比例转换为游戏坐标系中的位置
            return MapCoordinate.Main2048ToGame(new Point2f(BigMap.ScaleTo2048 * p.X, BigMap.ScaleTo2048 * p.Y));
        }
        else
        {
            throw new InvalidOperationException("当前不在地图界面"); // 如果没有找到地图缩放按钮，抛出异常
        }
    }

    /// <summary>
    /// 获取最近的传送点位置
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    /// <returns></returns>
    public (double x, double y) GetRecentlyTpPoint(double x, double y)
    {
        double recentX = 0; // 记录最近的 X 坐标
        double recentY = 0; // 记录最近的 Y 坐标
        var minDistance = double.MaxValue; // 初始化最小距离为最大值
        foreach (var tpPosition in MapAssets.Instance.TpPositions) // 遍历所有传送点位置
        {
            // 计算当前传送点与给定点的距离
            var distance = Math.Sqrt(Math.Pow(tpPosition.X - x, 2) + Math.Pow(tpPosition.Y - y, 2));
            if (distance < minDistance) // 如果当前距离小于最小距离
            {
                minDistance = distance; // 更新最小距离
                recentX = tpPosition.X; // 更新最近的 X 坐标
                recentY = tpPosition.Y; // 更新最近的 Y 坐标
            }
        }

        // 返回最近传送点的坐标
        return (recentX, recentY);
    }

    public async Task<bool> SwitchRecentlyCountryMap(double x, double y)
    {
        // 可能是地下地图，切换到地上地图
        using var ra2 = CaptureToRectArea(); // 捕获当前矩形区域
        if (Bv.BigMapIsUnderground(ra2)) // 判断当前地图是否为地下地图
        {
            ra2.Find(_assets.MapUndergroundToGroundButtonRo).Click(); // 找到并点击地下到地上的切换按钮
            await Delay(200, cts); // 等待 200 毫秒
        }

        // 识别当前位置
        var bigMapCenterPoint = GetPositionFromBigMap(); // 从大地图中获取当前位置坐标
        Logger.LogInformation("识别当前位置：{Pos}", bigMapCenterPoint); // 记录当前位置坐标

        // 计算当前位置到目标点的距离
        var minDistance = Math.Sqrt(Math.Pow(bigMapCenterPoint.X - x, 2) + Math.Pow(bigMapCenterPoint.Y - y, 2)); 
        var minCountry = "当前位置"; // 初始化最近国家为当前位置
        foreach (var (country, position) in MapAssets.Instance.CountryPositions) // 遍历国家位置
        {
            var distance = Math.Sqrt(Math.Pow(position[0] - x, 2) + Math.Pow(position[1] - y, 2)); // 计算当前位置到目标国家的距离
            if (distance < minDistance) // 如果距离小于当前最小距离
            {
                minDistance = distance; // 更新最小距离
                minCountry = country; // 更新最近国家
            }
        }

        Logger.LogInformation("离目标传送点最近的区域是：{Country}", minCountry); // 记录离目标传送点最近的区域
        if (minCountry != "当前位置") // 如果最近区域不是当前位置
        {
            GameCaptureRegion.GameRegionClick((rect, scale) => (rect.Width - 160 * scale, rect.Height - 60 * scale)); // 点击游戏区域
            await Delay(300, cts); // 等待 300 毫秒
            var ra = CaptureToRectArea(); // 再次捕获当前矩形区域
            var list = ra.FindMulti(new RecognitionObject // 在区域内进行多重识别
            {
                RecognitionType = RecognitionTypes.Ocr, // 使用 OCR 识别
                RegionOfInterest = new Rect(ra.Width / 2, 0, ra.Width / 2, ra.Height) // 识别区域为右半部分
            });
            list.FirstOrDefault(r => r.Text.Length == minCountry.Length && !r.Text.Contains("委托") && r.Text.Contains(minCountry))?.Click(); // 点击符合条件的区域
            Logger.LogInformation("切换到区域：{Country}", minCountry); // 记录已切换到的区域
            await Delay(500, cts); // 等待 500 毫秒
            return true; // 返回成功
        }

        return false; // 返回失败
    }

    public async Task Tp(string name)
    {
        // 通过大地图传送到指定传送点
    }

    public async Task TpByF1(string name)
    {
        // 传送到指定传送点
    }

    public async Task ClickTpPoint(ImageRegion imageRegion)
    {
        // 1.判断是否在地图界面
        if (Bv.IsInBigMapUi(imageRegion)) // 判断是否在大地图界面
        {
            // 2. 判断是否有传送按钮
            var hasTeleportButton = CheckTeleportButton(imageRegion); // 检查是否存在传送按钮

            if (!hasTeleportButton) // 如果没有传送按钮
            {
                // 存在地图关闭按钮，说明未选中传送点，直接返回
                var mapCloseRa = imageRegion.Find(_assets.MapCloseButtonRo); // 查找地图关闭按钮
                if (!mapCloseRa.IsEmpty()) // 如果找到地图关闭按钮
                {
                    return; // 返回
                }

                // 存在地图选择按钮，说明未选中传送点，直接返回
                var mapChooseRa = imageRegion.Find(_assets.MapChooseRo); // 查找地图选择按钮
                if (!mapChooseRa.IsEmpty()) // 如果找到地图选择按钮
                {
                    return; // 返回
                }

                // 3. 循环判断选项列表是否有传送点
                var hasMapChooseIcon = CheckMapChooseIcon(imageRegion); // 检查地图是否有选择图标
                if (hasMapChooseIcon) // 如果有选择图标
                {
                    await Delay(TaskContext.Instance().Config.QuickTeleportConfig.WaitTeleportPanelDelay, cts); // 等待指定时间
                    CheckTeleportButton(CaptureToRectArea()); // 再次检查传送按钮
                }
            }
        }
    }

    // 检查图像区域中是否存在传送按钮
    private bool CheckTeleportButton(ImageRegion imageRegion)
    {
        // 初始化变量，表示是否发现传送按钮
        var hasTeleportButton = false;
        // 在图像区域中查找传送按钮的图像
        imageRegion.Find(_assets.TeleportButtonRo, ra =>
        {
            // 如果找到传送按钮，点击按钮
            ra.Click();
            // 设置变量为 true，表示已找到传送按钮
            hasTeleportButton = true;
        });
        // 返回是否找到传送按钮
        return hasTeleportButton;
    }

    /// <summary>
    /// 全匹配一遍并进行文字识别
    /// 60ms ~200ms
    /// </summary>
    /// <param name="imageRegion"></param>
    /// <returns></returns>
    // 检查图像区域中是否存在地图选择图标
    private bool CheckMapChooseIcon(ImageRegion imageRegion)
    {
        // 初始化变量，表示是否发现地图选择图标
        var hasMapChooseIcon = false;

        // 在图像区域中匹配地图选择图标的模板
        var rResultList = MatchTemplateHelper.MatchMultiPicForOnePic(imageRegion.SrcGreyMat[_assets.MapChooseIconRoi], _assets.MapChooseIconGreyMatList);
        // 如果有匹配结果
        if (rResultList.Count > 0)
        {
            // 按高度排序匹配结果
            rResultList = [.. rResultList.OrderBy(x => x.Y)];
            // 遍历排序后的匹配结果
            foreach (var iconRect in rResultList)
            {
                // 提取图标区域并进行文字识别
                using var ra = imageRegion.DeriveCrop(_assets.MapChooseIconRoi.X + iconRect.X + iconRect.Width, _assets.MapChooseIconRoi.Y + iconRect.Y, 200, iconRect.Height);
                using var textRegion = ra.Find(new RecognitionObject
                {
                    RecognitionType = RecognitionTypes.ColorRangeAndOcr,
                    LowerColor = new Scalar(249, 249, 249), // 只取白色文字
                    UpperColor = new Scalar(255, 255, 255),
                });
                // 如果识别到的文字为空或只有一个字符，则跳过
                if (string.IsNullOrEmpty(textRegion.Text) || textRegion.Text.Length == 1)
                {
                    continue;
                }

                // 记录发现的地图选择图标并点击
                Logger.LogInformation("传送：点击 {Option}", textRegion.Text);
                Thread.Sleep(TaskContext.Instance().Config.QuickTeleportConfig.TeleportListClickDelay);
                ra.Click();
                // 设置变量为 true，表示已找到地图选择图标
                hasMapChooseIcon = true;
                // 跳出循环
                break;
            }
        }

        // 返回是否找到地图选择图标
        return hasMapChooseIcon;
    }
这段代码 `{` 是不完整的，可能需要更多上下文来正确注释。能否提供更完整的代码或上下文？
```