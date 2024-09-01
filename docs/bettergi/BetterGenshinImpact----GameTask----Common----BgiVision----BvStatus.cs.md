# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\BgiVision\BvStatus.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Common.Element.Assets; // 引用包含元素资产的命名空间
using BetterGenshinImpact.GameTask.Model.Area; // 引用包含区域模型的命名空间
using BetterGenshinImpact.GameTask.QuickTeleport.Assets; // 引用包含快捷传送资产的命名空间
using System; // 引用系统命名空间，以使用系统相关功能

namespace BetterGenshinImpact.GameTask.Common.BgiVision
{
    /// <summary>
    /// 模仿OpenCv的静态类
    /// 用于原神的各类识别与控制操作
    ///
    /// 此处主要是对游戏内的一些状态进行识别
    /// </summary>
    public static partial class Bv
    {
        public static string WhichGameUi()
        {
            throw new NotImplementedException(); // 抛出异常，表示该方法尚未实现
        }

        /// <summary>
        /// 是否在主界面
        /// </summary>
        /// <param name="captureRa"></param>
        /// <returns></returns>
        public static bool IsInMainUi(ImageRegion captureRa)
        {
            return captureRa.Find(ElementAssets.Instance.PaimonMenuRo).IsExist(); // 判断主界面的 Paimon 菜单是否存在来确定是否在主界面
        }

        /// <summary>
        /// 是否在大地图界面
        /// </summary>
        /// <param name="captureRa"></param>
        /// <returns></returns>
        public static bool IsInBigMapUi(ImageRegion captureRa)
        {
            return captureRa.Find(QuickTeleportAssets.Instance.MapScaleButtonRo).IsExist(); // 判断大地图界面的缩放按钮是否存在来确定是否在大地图界面
        }

        /// <summary>
        /// 大地图界面是否在地底
        /// 鼠标悬浮在地下图标或者处于切换动画的时候可能会误识别
        /// </summary>
        /// <param name="captureRa"></param>
        /// <returns></returns>
        public static bool BigMapIsUnderground(ImageRegion captureRa)
        {
            return captureRa.Find(QuickTeleportAssets.Instance.MapUndergroundSwitchButtonRo).IsExist(); // 判断地下切换按钮是否存在来确定大地图界面是否在地底
        }

        public static MotionStatus GetMotionStatus(ImageRegion captureRa)
        {
            var spaceExist = captureRa.Find(ElementAssets.Instance.SpaceKey).IsExist(); // 判断空格键是否存在
            var xExist = captureRa.Find(ElementAssets.Instance.XKey).IsExist(); // 判断 X 键是否存在
            if (spaceExist)
            {
                return xExist ? MotionStatus.Climb : MotionStatus.Fly; // 如果空格键存在且 X 键存在，返回 Climb，否则返回 Fly
            }
            else
            {
                return MotionStatus.Normal; // 如果空格键不存在，返回 Normal
            }
        }
    }

    public enum MotionStatus
    {
        Normal, // 正常状态
        Fly, // 飞行状态
        Climb, // 攀爬状态
    }
}
```