# `.\better-genshin-impact\BetterGenshinImpact\View\Drawable\VisionContext.cs`

```cs
#region 命名空间定义
﻿namespace BetterGenshinImpact.View.Drawable
{
    /// <summary>
    /// Vision 上下文类，处理视图相关的上下文信息
    /// </summary>
    public class VisionContext
    {
        // 存储 VisionContext 单例的静态字段
        private static VisionContext? _uniqueInstance;
        // 用于线程安全的锁对象
        private static readonly object Locker = new();

        // 私有构造函数，防止外部实例化
        private VisionContext()
        {
        }

        // 获取 VisionContext 单例的静态方法
        public static VisionContext Instance()
        {
            // 如果单例实例为空，则初始化
            if (_uniqueInstance == null)
            {
                // 锁定，以确保线程安全
                lock (Locker)
                {
                    // 双重检查实例是否为空，如果仍然为空则创建新实例
                    _uniqueInstance ??= new VisionContext();
                }
            }

            // 返回单例实例
            return _uniqueInstance;
        }

        // 注释掉的日志记录属性，可能用于记录日志
        //public ILogger? Log { get; set; }

        // Drawable 属性，标记是否可以绘制
        public bool Drawable { get; set; }

        // DrawContent 属性，表示绘制内容，默认为新的 DrawContent 实例
        public DrawContent DrawContent { get; set; } = new();
    }
}
```