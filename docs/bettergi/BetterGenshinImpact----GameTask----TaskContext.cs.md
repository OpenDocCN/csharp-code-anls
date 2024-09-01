# `.\better-genshin-impact\BetterGenshinImpact\GameTask\TaskContext.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间中的类
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.Core.Simulator 命名空间中的类
using BetterGenshinImpact.Core.Simulator;
# 引入 BetterGenshinImpact.GameTask.Model 命名空间中的类
using BetterGenshinImpact.GameTask.Model;
# 引入 BetterGenshinImpact.Genshin.Settings 命名空间中的类
using BetterGenshinImpact.Genshin.Settings;
# 引入 BetterGenshinImpact.Helpers 命名空间中的类
using BetterGenshinImpact.Helpers;
# 引入 BetterGenshinImpact.Service 命名空间中的类
using BetterGenshinImpact.Service;
# 引入系统命名空间中的类
using System;
# 引入线程命名空间中的类
using System.Threading;

# 定义 BetterGenshinImpact.GameTask 命名空间
namespace BetterGenshinImpact.GameTask
{
    # <summary>
    # 任务上下文
    # </summary>
    # 定义任务上下文类
    public class TaskContext
    {
        # 定义静态私有 TaskContext 实例
        private static TaskContext? _uniqueInstance;
        # 定义用于实例化的锁对象
        private static object? InstanceLocker;

        # 关闭警告，指示构造函数中必须包含非 null 值的字段
#pragma warning disable CS8618 // 在退出构造函数时，不可为 null 的字段必须包含非 null 值。请考虑声明为可以为 null。
        # 定义私有构造函数，防止外部实例化
        private TaskContext()
        {
        }
#pragma warning restore CS8618 // 在退出构造函数时，不可为 null 的字段必须包含非 null 值。请考虑声明为可以为 null。

        # 定义静态方法获取唯一实例
        public static TaskContext Instance()
        {
            # 确保 TaskContext 实例化并返回唯一实例
            return LazyInitializer.EnsureInitialized(ref _uniqueInstance, ref InstanceLocker, () => new TaskContext());
        }

        # 定义初始化方法
        public void Init(IntPtr hWnd)
        {
            # 设置游戏窗口句柄
            GameHandle = hWnd;
            # 初始化模拟器并发送消息
            PostMessageSimulator = Simulation.PostMessage(GameHandle);
            # 创建系统信息实例
            SystemInfo = new SystemInfo(hWnd);
            # 设置 DPI 缩放比例
            DpiScale = DpiHelper.ScaleY;
            # 初始化标志设置为 true
            IsInitialized = true;
        }

        # 定义是否已初始化属性
        public bool IsInitialized { get; set; }

        # 定义游戏窗口句柄属性
        public IntPtr GameHandle { get; set; }

        # 定义模拟器属性
        public PostMessageSimulator PostMessageSimulator { get; private set; }

        # 定义 DPI 缩放比例属性
        public float DpiScale { get; set; }

        # 定义系统信息属性
        public SystemInfo SystemInfo { get; set; }

        # 定义配置属性
        public AllConfig Config
        {
            get
            {
                # 如果配置未初始化，则抛出异常
                if (ConfigService.Config == null)
                {
                    throw new Exception("Config未初始化");
                }

                # 返回配置实例
                return ConfigService.Config;
            }
        }

        # 定义游戏设置属性
        public SettingsContainer? GameSettings { get; set; }

        # <summary>
        # 关联启动原神的时间
        # 注意 IsInitialized = false 时，这个值就会被设置
        # </summary>
        # 定义记录原神启动时间的属性，默认为最小值
        public DateTime LinkedStartGenshinTime { get; set; } = DateTime.MinValue;
    }
}
```