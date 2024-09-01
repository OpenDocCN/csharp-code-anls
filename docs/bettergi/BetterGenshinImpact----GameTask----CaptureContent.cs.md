# `.\better-genshin-impact\BetterGenshinImpact\GameTask\CaptureContent.cs`

```cs
# 引入 BetterGenshinImpact.GameTask.Model.Area 命名空间
using BetterGenshinImpact.GameTask.Model.Area;
# 引入 System 命名空间
using System;
# 引入 System.Drawing 命名空间
using System.Drawing;

namespace BetterGenshinImpact.GameTask
{
    /// <summary>
    /// 捕获的内容
    /// 以及一些多个trigger会用到的内容
    /// </summary>
    public class CaptureContent : IDisposable
    {
        # 定义最大帧索引值
        public static readonly int MaxFrameIndexSecond = 60;
        # 获取源位图
        public Bitmap SrcBitmap { get; }
        # 获取当前帧索引
        public int FrameIndex { get; }
        # 获取计时器间隔
        public double TimerInterval { get; }

        # 计算帧率
        public int FrameRate => (int)(1000 / TimerInterval);

        # 获取捕获区域
        public ImageRegion CaptureRectArea { get; private set; }

        # 构造函数，初始化 CaptureContent 对象
        public CaptureContent(Bitmap srcBitmap, int frameIndex, double interval)
        {
            SrcBitmap = srcBitmap;
            FrameIndex = frameIndex;
            TimerInterval = interval;
            # 获取系统信息
            var systemInfo = TaskContext.Instance().SystemInfo;

            # 计算游戏捕获区域
            var gameCaptureRegion = systemInfo.DesktopRectArea.Derive(srcBitmap, systemInfo.CaptureAreaRect.X, systemInfo.CaptureAreaRect.Y);
            # 将捕获区域转换为 1080P
            CaptureRectArea = gameCaptureRegion.DeriveTo1080P();
        }

        # 释放资源
        public void Dispose()
        {
            CaptureRectArea.Dispose();
            SrcBitmap.Dispose();
        }
    }
}
```