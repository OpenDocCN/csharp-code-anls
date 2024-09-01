# `.\better-genshin-impact\BetterGenshinImpact\GameTask\Common\BgiVision\BvSimpleOperation.cs`

```cs
﻿using BetterGenshinImpact.GameTask.Common.Element.Assets;  // 引用包含游戏元素资产的命名空间
using BetterGenshinImpact.GameTask.Model.Area;  // 引用包含区域模型的命名空间

namespace BetterGenshinImpact.GameTask.Common.BgiVision  // 定义一个命名空间，组织相关功能的类
{
    /// <summary>
    /// 模仿OpenCv的静态类
    /// 用于原神的各类识别与控制操作
    ///
    /// 此处主要是对游戏内的一些状态进行识别
    /// </summary>
    public static partial class Bv  // 定义一个静态类 Bv，用于游戏控制和识别
    {
        /// <summary>
        /// 点击白色确认按钮
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickWhiteConfirmButton(ImageRegion captureRa)  // 定义一个方法，点击白色确认按钮
        {
            var ra = captureRa.Find(ElementAssets.Instance.BtnWhiteConfirm);  // 在给定区域中查找白色确认按钮
            if (ra.IsExist())  // 检查按钮是否存在
            {
                ra.Click();  // 点击按钮
                return true;  // 返回 true 表示点击成功
            }
            return false;  // 返回 false 表示点击失败
        }

        /// <summary>
        /// 点击白色取消按钮
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickWhiteCancelButton(ImageRegion captureRa)  // 定义一个方法，点击白色取消按钮
        {
            var ra = captureRa.Find(ElementAssets.Instance.BtnWhiteCancel);  // 在给定区域中查找白色取消按钮
            if (ra.IsExist())  // 检查按钮是否存在
            {
                ra.Click();  // 点击按钮
                return true;  // 返回 true 表示点击成功
            }
            return false;  // 返回 false 表示点击失败
        }

        /// <summary>
        /// 点击黑色确认按钮
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickBlackConfirmButton(ImageRegion captureRa)  // 定义一个方法，点击黑色确认按钮
        {
            var ra = captureRa.Find(ElementAssets.Instance.BtnBlackConfirm);  // 在给定区域中查找黑色确认按钮
            if (ra.IsExist())  // 检查按钮是否存在
            {
                ra.Click();  // 点击按钮
                return true;  // 返回 true 表示点击成功
            }
            return false;  // 返回 false 表示点击失败
        }

        /// <summary>
        /// 点击黑色取消按钮
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickBlackCancelButton(ImageRegion captureRa)  // 定义一个方法，点击黑色取消按钮
        {
            var ra = captureRa.Find(ElementAssets.Instance.BtnBlackCancel);  // 在给定区域中查找黑色取消按钮
            if (ra.IsExist())  // 检查按钮是否存在
            {
                ra.Click();  // 点击按钮
                return true;  // 返回 true 表示点击成功
            }
            return false;  // 返回 false 表示点击失败
        }

        /// <summary>
        /// 点击联机确认按钮
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickOnlineYesButton(ImageRegion captureRa)  // 定义一个方法，点击联机确认按钮
        {
            var ra = captureRa.Find(ElementAssets.Instance.BtnOnlineYes);  // 在给定区域中查找联机确认按钮
            if (ra.IsExist())  // 检查按钮是否存在
            {
                ra.Click();  // 点击按钮
                return true;  // 返回 true 表示点击成功
            }
            return false;  // 返回 false 表示点击失败
        }

        /// <summary>
        /// 点击联机取消按钮
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickOnlineNoButton(ImageRegion captureRa)  // 定义一个方法，点击联机取消按钮
        {
            var ra = captureRa.Find(ElementAssets.Instance.BtnOnlineNo);  // 在给定区域中查找联机取消按钮
            if (ra.IsExist())  // 检查按钮是否存在
            {
                ra.Click();  // 点击按钮
                return true;  // 返回 true 表示点击成功
            }
            return false;  // 返回 false 表示点击失败
        }

        /// <summary>
        /// 点击确认按钮（优先点击白色背景的确认按钮）
        /// </summary>
        /// <param name="captureRa">需要操作的区域</param>
        /// <returns>点击成功返回 true，否则返回 false</returns>
        public static bool ClickConfirmButton(ImageRegion captureRa)  // 定义一个方法，点击确认按钮（优先白色背景）
        {
            return ClickBlackConfirmButton(captureRa) || ClickWhiteConfirmButton(captureRa) || ClickOnlineYesButton(captureRa);  // 尝试点击黑色确认按钮、白色确认按钮或联机确认按钮，直到成功
        }
    }
}
    /// <summary>
    /// 点击取消按钮（优先点击白色背景的确认按钮）
    /// </summary>
    /// <param name="captureRa"></param>
    /// <returns></returns>
    // 定义一个静态方法，用于点击取消按钮，优先点击白色背景的确认按钮
    public static bool ClickCancelButton(ImageRegion captureRa)
    {
        // 尝试点击黑色背景的取消按钮，如果点击成功则返回 true
        return ClickBlackCancelButton(captureRa) || 
               // 如果黑色背景的取消按钮点击失败，尝试点击白色背景的取消按钮，如果点击成功则返回 true
               ClickWhiteCancelButton(captureRa) || 
               // 如果前两种按钮点击失败，最后尝试点击在线“否”按钮，如果点击成功则返回 true
               ClickOnlineNoButton(captureRa);
    }
# 代码块结束标志，表示当前代码块的结束
}
```