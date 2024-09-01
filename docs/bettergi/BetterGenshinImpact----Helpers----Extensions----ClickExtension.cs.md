# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\ClickExtension.cs`

```cs
# 引入 BetterGenshinImpact.Core.Simulator 命名空间
using BetterGenshinImpact.Core.Simulator;
# 引入 Fischless.WindowsInput 命名空间
using Fischless.WindowsInput;
# 引入 OpenCvSharp 命名空间
using OpenCvSharp;

# 定义 BetterGenshinImpact.Helpers.Extensions 命名空间
namespace BetterGenshinImpact.Helpers.Extensions;

# 定义一个静态扩展类 ClickExtension
public static class ClickExtension
{
    # 定义一个扩展方法 Click，接收一个 Point 类型的参数
    public static void Click(this Point point)
    {
        # 使用 Simulation.SendInput 发送鼠标移动到指定位置，计算方式是将屏幕坐标归一化到 [0, 65535] 范围内
        Simulation.SendInput.Mouse.MoveMouseTo(point.X * 65535 * 1d / PrimaryScreen.WorkingArea.Width,
            point.Y * 65535 * 1d / PrimaryScreen.WorkingArea.Height)
            # 模拟鼠标左键按下操作
            .LeftButtonDown()
            # 等待 50 毫秒
            .Sleep(50)
            # 模拟鼠标左键抬起操作
            .LeftButtonUp();
    }

    # 被注释掉的扩展方法 ClickCenter，接收一个 Rect 类型的参数和一个可选的布尔参数 isRand
    # public static void ClickCenter(this Rect rect, bool isRand = false)
    # {
    #     Simulation.SendInputEx.Mouse.MoveMouseTo((rect.X + (isRand ? Rd.Next(rect.Width) : rect.Width * 1d / 2)) * 65535 / PrimaryScreen.WorkingArea.Width,
    #         (rect.Y + (isRand ? Rd.Next(rect.Height) : rect.Height * 1d / 2)) * 65535 / PrimaryScreen.WorkingArea.Height)
    #         .LeftButtonDown()
    #         .Sleep(50)
    #         .LeftButtonUp();
    # }

    # 定义一个扩展方法 Click，接收两个 double 类型的参数 x 和 y
    public static IMouseSimulator Click(double x, double y)
    {
        # 使用 Simulation.SendInput 发送鼠标移动到指定位置，计算方式是将屏幕坐标归一化到 [0, 65535] 范围内
        return Simulation.SendInput.Mouse.MoveMouseTo(x * 65535 * 1d / PrimaryScreen.WorkingArea.Width,
            y * 65535 * 1d / PrimaryScreen.WorkingArea.Height)
            # 模拟鼠标左键按下操作
            .LeftButtonDown()
            # 等待 50 毫秒
            .Sleep(50)
            # 模拟鼠标左键抬起操作
            .LeftButtonUp();
    }

    # 定义一个扩展方法 Move，接收两个 double 类型的参数 x 和 y
    public static IMouseSimulator Move(double x, double y)
    {
        # 使用 Simulation.SendInput 发送鼠标移动到指定位置，计算方式是将屏幕坐标归一化到 [0, 65535] 范围内
        return Simulation.SendInput.Mouse.MoveMouseTo(x * 65535 * 1d / PrimaryScreen.WorkingArea.Width,
            y * 65535 * 1d / PrimaryScreen.WorkingArea.Height);
    }

    # 定义一个扩展方法 Move，接收一个 Point 类型的参数
    public static IMouseSimulator Move(Point p)
    {
        # 调用 Move(double x, double y) 方法，将 Point 的 x 和 y 坐标传入
        return Move(p.X, p.Y);
    }
}
```