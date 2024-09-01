# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoFight\Model\Avatar.cs`

```cs
# 引用相关的命名空间
﻿using BetterGenshinImpact.Core.Recognition.OCR;  # 引入OCR识别相关的命名空间
using BetterGenshinImpact.Core.Recognition.OpenCv;  # 引入OpenCV图像处理相关的命名空间
using BetterGenshinImpact.Core.Simulator;  # 引入模拟器相关的命名空间
using BetterGenshinImpact.GameTask.AutoFight.Config;  # 引入自动战斗配置相关的命名空间
using BetterGenshinImpact.GameTask.Model.Area;  # 引入区域模型相关的命名空间
using BetterGenshinImpact.Helpers;  # 引入帮助类相关的命名空间
using Microsoft.Extensions.Logging;  # 引入日志记录相关的命名空间
using OpenCvSharp;  # 引入OpenCVSharp库相关的命名空间
using System;  # 引入系统基础类相关的命名空间
using System.Linq;  # 引入LINQ查询相关的命名空间
using System.Threading;  # 引入线程相关的命名空间
using Vanara.PInvoke;  # 引入Vanara库的PInvoke相关命名空间
using static BetterGenshinImpact.GameTask.Common.TaskControl;  # 引入TaskControl类中的静态成员

namespace BetterGenshinImpact.GameTask.AutoFight.Model;  # 定义命名空间

/// <summary>
/// 队伍内的角色
/// </summary>
public class Avatar  # 定义Avatar类
{
    /// <summary>
    /// 角色名称 中文
    /// </summary>
    public string Name { get; set; }  # 角色的中文名称属性

    /// <summary>
    /// 角色名称 英文
    /// </summary>
    public string? NameEn { get; set; }  # 角色的英文名称属性（可为空）

    /// <summary>
    /// 队伍内序号
    /// </summary>
    public int Index { get; set; }  # 角色在队伍中的序号属性

    /// <summary>
    /// 武器类型
    /// </summary>
    public string Weapon { get; set; }  # 角色使用的武器类型属性

    /// <summary>
    /// 元素战技CD
    /// </summary>
    public double SkillCd { get; set; }  # 角色的元素战技冷却时间属性

    /// <summary>
    /// 长按元素战技CD
    /// </summary>
    public double SkillHoldCd { get; set; }  # 角色长按元素战技的冷却时间属性

    /// <summary>
    /// 元素爆发CD
    /// </summary>
    public double BurstCd { get; set; }  # 角色的元素爆发冷却时间属性

    /// <summary>
    /// 元素爆发是否就绪
    /// </summary>
    public bool IsBurstReady { get; set; }  # 角色的元素爆发是否就绪的状态属性

    /// <summary>
    /// 名字所在矩形位置
    /// </summary>
    public Rect NameRect { get; set; }  # 角色名称所在的矩形区域属性

    /// <summary>
    /// 名字右边的编号位置
    /// </summary>
    public Rect IndexRect { get; set; }  # 角色编号所在的矩形区域属性

    /// <summary>
    /// 任务取消令牌
    /// </summary>
    public CancellationTokenSource? Cts { get; set; }  # 任务取消令牌属性（可为空）

    /// <summary>
    /// 战斗场景
    /// </summary>
    public CombatScenes CombatScenes { get; set; }  # 战斗场景属性

    public Avatar(CombatScenes combatScenes, string name, int index, Rect nameRect)  # Avatar类的构造函数
    {
        CombatScenes = combatScenes;  # 初始化战斗场景属性
        Name = name;  # 初始化角色名称属性
        Index = index;  # 初始化角色序号属性
        NameRect = nameRect;  # 初始化角色名称矩形区域属性

        var ca = DefaultAutoFightConfig.CombatAvatarMap[name];  # 从默认配置中获取角色相关信息
        NameEn = ca.NameEn;  # 初始化角色英文名称属性
        Weapon = ca.Weapon;  # 初始化角色武器类型属性
        SkillCd = ca.SkillCd;  # 初始化角色元素战技冷却时间属性
        SkillHoldCd = ca.SkillHoldCd;  # 初始化角色长按元素战技冷却时间属性
        BurstCd = ca.BurstCd;  # 初始化角色元素爆发冷却时间属性
    }

    /// <summary>
    /// 是否存在角色被击败
    /// 通过判断确认按钮
    /// </summary>
    /// <param name="region"></param>
    /// <returns></returns>
    public void ThrowWhenDefeated(ImageRegion region)  # 检查角色是否被击败的方法
    {
        using var confirmRectArea = region.Find(AutoFightContext.Instance.FightAssets.ConfirmRa);  # 查找确认按钮区域
        if (!confirmRectArea.IsEmpty())  # 如果确认按钮区域不为空
        {
            Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_ESCAPE);  # 发送按键事件：按下ESC键
            Sleep(600, Cts);  # 等待600毫秒
            Simulation.SendInput.Keyboard.KeyPress(User32.VK.VK_M);  # 发送按键事件：按下M键
            throw new Exception("存在角色被击败，按 M 键打开地图，并停止自动秘境。");  # 抛出异常并停止自动秘境
        }
    }

    /// <summary>
    /// 切换到本角色
    /// 切换cd是1秒，如果切换失败，会尝试再次切换，最多尝试5次
    /// </summary>
    public void Switch()  # 切换到本角色的方法
    # 循环执行 30 次
    {
        for (var i = 0; i < 30; i++)
        {
            # 如果 Cts 对象的取消请求属性为 true，则退出方法
            if (Cts is { IsCancellationRequested: true })
            {
                return;
            }

            # 捕获区域并存储
            var region = CaptureToRectArea();
            # 如果角色失败，抛出异常
            ThrowWhenDefeated(region);

            # 计算当前场景中未激活的角色数量
            var notActiveCount = CombatScenes.Avatars.Count(avatar => !avatar.IsActive(region));
            # 如果区域处于激活状态并且未激活的角色数量为 3，则退出方法
            if (IsActive(region) && notActiveCount == 3)
            {
                return;
            }

            # 模拟按下键盘上的一个键
            AutoFightContext.Instance.Simulator.KeyPress(User32.VK.VK_1 + (byte)Index - 1);
            # 暂停 250 毫秒
            Sleep(250, Cts);
        }
    }

    /// <summary>
    /// 切换到本角色
    /// 切换 cd 是 1 秒，如果切换失败，会尝试再次切换，最多尝试 5 次
    /// </summary>
    public void SwitchWithoutCts()
    {
        # 循环执行 10 次
        for (var i = 0; i < 10; i++)
        {
            # 捕获区域并存储
            var region = CaptureToRectArea();
            # 如果角色失败，抛出异常
            ThrowWhenDefeated(region);

            # 计算当前场景中未激活的角色数量
            var notActiveCount = CombatScenes.Avatars.Count(avatar => !avatar.IsActive(region));
            # 如果区域处于激活状态并且未激活的角色数量为 3，则退出方法
            if (IsActive(region) && notActiveCount == 3)
            {
                return;
            }

            # 模拟按下键盘上的一个键
            AutoFightContext.Instance.Simulator.KeyPress(User32.VK.VK_1 + (byte)Index - 1);
            # 暂停 250 毫秒
            Sleep(250);
        }
    }

    /// <summary>
    /// 是否出战状态
    /// </summary>
    /// <returns></returns>
    public bool IsActive(ImageRegion region)
    {
        # 如果 IndexRect 为空，则抛出异常
        if (IndexRect == Rect.Empty)
        {
            throw new Exception("IndexRect为空");
        }
        else
        {
            # 从区域中剪裁出 IndexRect 区域
            var indexRa = region.DeriveCrop(IndexRect);
            # 计算剪裁区域中灰度值在 251 到 255 之间的像素点数量
            var count = OpenCvCommonHelper.CountGrayMatColor(indexRa.SrcGreyMat, 251, 255);
            # 如果这些像素点的比例大于 0.5，则返回 false；否则返回 true
            if (count * 1.0 / (IndexRect.Width * IndexRect.Height) > 0.5)
            {
                return false;
            }
            else
            {
                return true;
            }
        }
    }

    /// <summary>
    /// 是否出战状态
    /// </summary>
    /// <returns></returns>
    [Obsolete]
    public bool IsActiveNoIndexRect(ImageRegion region)
    {
        // 通过寻找右侧人物编号来判断是否出战
        if (IndexRect == Rect.Empty)
        {
            // 获取系统资产缩放比例
            var assetScale = TaskContext.Instance().SystemInfo.AssetScale;
            // 从区域中剪裁出队伍区域
            var teamRa = region.DeriveCrop(AutoFightContext.Instance.FightAssets.TeamRect);
            // 计算出 block 区域的 X 坐标，并创建新的矩形区域
            var blockX = NameRect.X + NameRect.Width * 2 - 10;
            var block = teamRa.DeriveCrop(new Rect(blockX, NameRect.Y, teamRa.Width - blockX, NameRect.Height * 2));
            // Cv2.ImWrite($"block_{Name}.png", block.SrcMat); // 注释掉：保存当前 block 区域图像
            // 取白色区域
            var bMat = OpenCvCommonHelper.Threshold(block.SrcMat, new Scalar(255, 255, 255), new Scalar(255, 255, 255));
            // Cv2.ImWrite($"block_b_{Name}.png", bMat); // 注释掉：保存阈值处理后的图像
            // 识别图像中的轮廓
            Cv2.FindContours(bMat, out var contours, out _, RetrievalModes.External,
                ContourApproximationModes.ApproxSimple);
            // 如果找到轮廓
            if (contours.Length > 0)
            {
                // 计算每个轮廓的边界矩形，并筛选出符合条件的矩形
                var boxes = contours.Select(Cv2.BoundingRect).Where(w => w.Width >= 20 * assetScale && w.Height >= 18 * assetScale).OrderByDescending(w => w.Width).ToList();
                // 如果有符合条件的矩形
                if (boxes.Count is not 0)
                {
                    // 取第一个矩形作为 IndexRect
                    IndexRect = boxes.First();
                    return false;
                }
            }
        }
        else
        {
            // 剪裁出 IndexRect 区域
            var teamRa = region.DeriveCrop(AutoFightContext.Instance.FightAssets.TeamRect);
            var blockX = NameRect.X + NameRect.Width * 2 - 10;
            var indexBlock = teamRa.DeriveCrop(new Rect(blockX + IndexRect.X, NameRect.Y + IndexRect.Y, IndexRect.Width, IndexRect.Height));
            // Cv2.ImWrite($"indexBlock_{Name}.png", indexBlock.SrcMat); // 注释掉：保存剪裁后的图像
            // 计算 IndexRect 区域中灰度值为 255 的像素数量
            var count = OpenCvCommonHelper.CountGrayMatColor(indexBlock.SrcGreyMat, 255);
            // 如果白色区域占比大于 50%
            if (count * 1.0 / (IndexRect.Width * IndexRect.Height) > 0.5)
            {
                return false;
            }
        }

        // 记录日志，说明当前出战状态
        Logger.LogInformation("{Name} 当前出战", Name);
        return true;
    }

    /// <summary>
    /// 普通攻击
    /// </summary>
    /// <param name="ms">攻击时长，建议是200的倍数</param>
    public void Attack(int ms = 0)
    {
        // 当攻击时间大于等于0时循环
        while (ms >= 0)
        {
            // 如果 Cts 被取消请求，退出方法
            if (Cts is { IsCancellationRequested: true })
            {
                return;
            }

            // 模拟点击左键
            AutoFightContext.Instance.Simulator.LeftButtonClick();
            // 减少攻击时间
            ms -= 200;
            // 暂停200毫秒
            Sleep(200, Cts);
        }
    }

    /// <summary>
    /// 使用元素战技 E
    /// </summary>
    public void UseSkill(bool hold = false)
    {
        # 遍历循环，循环次数为 1 次
        for (var i = 0; i < 1; i++)
        {
            # 如果 Cts 被取消请求，则退出方法
            if (Cts is { IsCancellationRequested: true })
            {
                return;
            }

            # 如果 hold 为 true
            if (hold)
            {
                # 如果 Name 等于 "纳西妲"
                if (Name == "纳西妲")
                {
                    # 模拟按下 E 键
                    AutoFightContext.Instance.Simulator.KeyDown(User32.VK.VK_E);
                    # 等待 300 毫秒，不受 Cts 影响
                    Sleep(300, Cts);
                    # 循环 10 次
                    for (int j = 0; j < 10; j++)
                    {
                        # 鼠标移动 1000 像素到右侧
                        Simulation.SendInput.Mouse.MoveMouseBy(1000, 0);
                        # 等待 50 毫秒，不受 Cts 影响
                        Sleep(50); // 持续操作不应该被cts取消
                    }

                    # 等待 300 毫秒，不受 Cts 影响
                    Sleep(300); // 持续操作不应该被cts取消
                    # 模拟释放 E 键
                    AutoFightContext.Instance.Simulator.KeyUp(User32.VK.VK_E);
                }
                else
                {
                    # 长按 E 键
                    AutoFightContext.Instance.Simulator.LongKeyPress(User32.VK.VK_E);
                }
            }
            else
            {
                # 单次按下 E 键
                AutoFightContext.Instance.Simulator.KeyPress(User32.VK.VK_E);
            }

            # 等待 200 毫秒，不受 Cts 影响
            Sleep(200, Cts);

            # 捕获当前区域的图像
            var region = CaptureToRectArea();
            # 检查区域内是否被击败
            ThrowWhenDefeated(region);
            # 获取技能当前的冷却时间
            var cd = GetSkillCurrentCd(region);
            # 如果冷却时间大于 0
            if (cd > 0)
            {
                # 记录冷却时间
                Logger.LogInformation(hold ? "{Name} 长按元素战技，cd:{Cd}" : "{Name} 点按元素战技，cd:{Cd}", Name, cd);
                # todo 把cd加入执行队列
                return;
            }
        }
    }

    /// <summary>
    /// 元素战技是否正在CD中
    /// 右下 267x132
    /// 77x77
    /// </summary>
    public double GetSkillCurrentCd(ImageRegion imageRegion)
    {
        # 从图像区域中裁剪出技能冷却区域
        var eRa = imageRegion.DeriveCrop(AutoFightContext.Instance.FightAssets.ERect);
        # 使用 OCR 读取裁剪区域中的文字
        var text = OcrFactory.Paddle.Ocr(eRa.SrcGreyMat);
        # 尝试解析读取到的文字为双精度数值
        return StringUtils.TryParseDouble(text);
    }

    /// <summary>
    /// 使用元素爆发 Q
    /// Q释放等待 2s 超时认为没有Q技能
    /// </summary>
    public void UseBurst()
    {
        # 循环 10 次
        for (var i = 0; i < 10; i++)
        {
            # 如果 Cts 被取消请求，则退出方法
            if (Cts is { IsCancellationRequested: true })
            {
                return;
            }

            # 模拟按下 Q 键
            AutoFightContext.Instance.Simulator.KeyPress(User32.VK.VK_Q);
            # 等待 200 毫秒，不受 Cts 影响
            Sleep(200, Cts);

            # 捕获当前区域的图像
            var region = CaptureToRectArea();
            # 检查区域内是否被击败
            ThrowWhenDefeated(region);
            # 统计区域内不活跃的头像数量
            var notActiveCount = CombatScenes.Avatars.Count(avatar => !avatar.IsActive(region));
            # 如果所有头像都活跃
            if (notActiveCount == 0)
            {
                # 等待 1500 毫秒，不受 Cts 影响
                Sleep(1500, Cts);
                return;
            }
        }
    }
    // /// <summary>
    // /// 元素爆发是否正在CD中
    // /// 右下 157x165
    // /// 110x110
    // /// </summary>
    // public double GetBurstCurrentCd(CaptureContent content)
    // {
    //     // 截取特定区域的图像
    //     var qRa = content.CaptureRectArea.Crop(AutoFightContext.Instance.FightAssets.QRect);
    //     // 使用 OCR 识别图像中的文本
    //     var text = OcrFactory.Paddle.Ocr(qRa.SrcGreyMat);
    //     // 将识别的文本转换为双精度浮点数
    //     return StringUtils.TryParseDouble(text);
    // }

    /// <summary>
    /// 冲刺
    /// </summary>
    public void Dash(int ms = 0)
    {
        // 如果取消标志被请求，直接返回
        if (Cts is { IsCancellationRequested: true })
        {
            return;
        }

        // 如果未指定时间，默认为 200 毫秒
        if (ms == 0)
        {
            ms = 200;
        }

        // 按下右键以启动冲刺
        AutoFightContext.Instance.Simulator.RightButtonDown();
        // 等待指定时间
        Sleep(ms); // 冲刺不能被cts取消
        // 松开右键结束冲刺
        AutoFightContext.Instance.Simulator.RightButtonUp();
    }

    public void Walk(string key, int ms)
    {
        // 如果取消标志被请求，直接返回
        if (Cts is { IsCancellationRequested: true })
        {
            return;
        }

        // 根据按键设置虚拟键码
        User32.VK vk = User32.VK.VK_NONAME;
        if (key == "w")
        {
            vk = User32.VK.VK_W;
        }
        else if (key == "s")
        {
            vk = User32.VK.VK_S;
        }
        else if (key == "a")
        {
            vk = User32.VK.VK_A;
        }
        else if (key == "d")
        {
            vk = User32.VK.VK_D;
        }

        // 如果未识别的按键，直接返回
        if (vk == User32.VK.VK_NONAME)
        {
            return;
        }

        // 按下指定按键以开始行走
        AutoFightContext.Instance.Simulator.KeyDown(vk);
        // 等待指定时间
        Sleep(ms); // 行走不能被cts取消
        // 松开指定按键以结束行走
        AutoFightContext.Instance.Simulator.KeyUp(vk);
    }

    /// <summary>
    /// 移动摄像机
    /// </summary>
    /// <param name="pixelDeltaX">负数是左移，正数是右移</param>
    /// <param name="pixelDeltaY"></param>
    public void MoveCamera(int pixelDeltaX, int pixelDeltaY)
    {
        // 移动鼠标以调整摄像机位置
        Simulation.SendInput.Mouse.MoveMouseBy(pixelDeltaX, pixelDeltaY);
    }

    /// <summary>
    /// 等待
    /// </summary>
    /// <param name="ms"></param>
    public void Wait(int ms)
    {
        // 等待指定时间，宏操作期间不应被取消
        Sleep(ms); // 由于存在宏操作，等待不应被cts取消
    }

    /// <summary>
    /// 跳跃
    /// </summary>
    public void Jump()
    {
        // 按下空格键以执行跳跃
        AutoFightContext.Instance.Simulator.KeyPress(User32.VK.VK_SPACE);
    }

    /// <summary>
    /// 重击
    /// </summary>
    public void Charge(int ms = 0)
    # 如果 ms 等于 0，设置 ms 为 1000
    if (ms == 0)
    {
        ms = 1000;
    }

    # 如果 Name 等于 "那维莱特"
    if (Name == "那维莱特")
    {
        # 按下左键
        AutoFightContext.Instance.Simulator.LeftButtonDown();
        # 当 ms 大于等于 0 时循环
        while (ms >= 0)
        {
            # 如果 Cts 对象的 IsCancellationRequested 属性为 true，退出方法
            if (Cts is { IsCancellationRequested: true })
            {
                return;
            }

            # 鼠标移动指定偏移量
            Simulation.SendInput.Mouse.MoveMouseBy(1000, 0);
            # 减少 ms 值
            ms -= 50;
            # 休眠 50 毫秒
            Sleep(50); // 持续操作不应该被cts取消
        }

        # 松开左键
        AutoFightContext.Instance.Simulator.LeftButtonUp();
    }
    else
    {
        # 按下左键
        AutoFightContext.Instance.Simulator.LeftButtonDown();
        # 休眠指定的 ms 时间
        Sleep(ms); // 持续操作不应该被cts取消
        # 松开左键
        AutoFightContext.Instance.Simulator.LeftButtonUp();
    }
}

# 根据指定的键值按下鼠标按钮
public void MouseDown(string key = "left")
{
    # 将键值转换为小写
    key = key.ToLower();
    # 如果键值为 "left"
    if (key == "left")
    {
        # 按下左键
        AutoFightContext.Instance.Simulator.LeftButtonDown();
    }
    # 如果键值为 "right"
    else if (key == "right")
    {
        # 按下右键
        AutoFightContext.Instance.Simulator.RightButtonDown();
    }
    # 如果键值为 "middle"
    else if (key == "middle")
    {
        # 按下中键
        Simulation.SendInput.Mouse.MiddleButtonDown();
    }
}

# 根据指定的键值松开鼠标按钮
public void MouseUp(string key = "left")
{
    # 将键值转换为小写
    key = key.ToLower();
    # 如果键值为 "left"
    if (key == "left")
    {
        # 松开左键
        AutoFightContext.Instance.Simulator.LeftButtonUp();
    }
    # 如果键值为 "right"
    else if (key == "right")
    {
        # 松开右键
        AutoFightContext.Instance.Simulator.RightButtonUp();
    }
    # 如果键值为 "middle"
    else if (key == "middle")
    {
        # 松开中键
        Simulation.SendInput.Mouse.MiddleButtonUp();
    }
}

# 根据指定的键值点击鼠标按钮
public void Click(string key = "left")
{
    # 将键值转换为小写
    key = key.ToLower();
    # 如果键值为 "left"
    if (key == "left")
    {
        # 点击左键
        AutoFightContext.Instance.Simulator.LeftButtonClick();
    }
    # 如果键值为 "right"
    else if (key == "right")
    {
        # 点击右键
        AutoFightContext.Instance.Simulator.RightButtonClick();
    }
    # 如果键值为 "middle"
    else if (key == "middle")
    {
        # 点击中键
        Simulation.SendInput.Mouse.MiddleButtonClick();
    }
}

# 根据指定的 x 和 y 偏移量移动鼠标
public void MoveBy(int x, int y)
{
    # 移动鼠标到指定的 x 和 y 偏移量
    Simulation.SendInput.Mouse.MoveMouseBy(x, y);
}

# 根据指定的键值按下键盘键
public void KeyDown(string key)
{
    # 将键值转换为虚拟键码
    var vk = User32Helper.ToVk(key);
    # 按下指定的键
    AutoFightContext.Instance.Simulator.KeyDown(vk);
}

# 根据指定的键值松开键盘键
public void KeyUp(string key)
{
    # 将键值转换为虚拟键码
    var vk = User32Helper.ToVk(key);
    # 松开指定的键
    AutoFightContext.Instance.Simulator.KeyUp(vk);
}

# 根据指定的键值按下并松开键盘键
public void KeyPress(string key)
{
    # 将键值转换为虚拟键码
    var vk = User32Helper.ToVk(key);
    # 按下并松开指定的键
    AutoFightContext.Instance.Simulator.KeyPress(vk);
}
# 结束一个代码块（可能是一个函数、条件语句、循环等的结束）
}
```