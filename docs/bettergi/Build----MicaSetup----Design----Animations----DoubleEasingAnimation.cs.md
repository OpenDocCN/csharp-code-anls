# `.\better-genshin-impact\Build\MicaSetup\Design\Animations\DoubleEasingAnimation.cs`

```cs
﻿using System;

namespace MicaSetup.Controls.Animations;

// 定义一个委托，表示一个双精度浮点数的缓动函数
public delegate double DoubleEasingAnimation(double t, double b, double c, double d);

public static class DoubleEasingAnimations
{
    // 实现一个双精度浮点数的 EaseInOutQuad 缓动函数
    public static double EaseInOutQuad(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 当时间在动画的一半时间内
        if ((t /= d / 2d) < 1d)
        {
            // 计算并返回 EaseInQuad 曲线的结果
            return c / 2d * t * t + b;
        }
        // 计算并返回 EaseOutQuad 曲线的结果
        return -c / 2d * ((t -= 1d) * (t - 2d) - 1d) + b;
    }

    // 实现一个双精度浮点数的 EaseInQuad 缓动函数
    public static double EaseInQuad(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseInQuad 曲线的结果
        return c * (t /= d) * t + b;
    }

    // 实现一个双精度浮点数的 EaseOutQuad 缓动函数
    public static double EaseOutQuad(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseOutQuad 曲线的结果
        return -c * (t /= d) * (t - 2d) + b;
    }

    // 实现一个双精度浮点数的 EaseInCubic 缓动函数
    public static double EaseInCubic(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseInCubic 曲线的结果
        return c * (t /= d) * t * t + b;
    }

    // 实现一个双精度浮点数的 EaseOutCubic 缓动函数
    public static double EaseOutCubic(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseOutCubic 曲线的结果
        return c * ((t = t / d - 1d) * t * t + 1d) + b;
    }

    // 实现一个双精度浮点数的 EaseInOutCubic 缓动函数
    public static double EaseInOutCubic(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 当时间在动画的一半时间内
        if ((t /= d / 2d) < 1d)
        {
            // 计算并返回 EaseInCubic 曲线的结果
            return c / 2d * t * t * t + b;
        }
        // 计算并返回 EaseOutCubic 曲线的结果
        return c / 2d * ((t -= 2d) * t * t + 2d) + b;
    }

    // 实现一个双精度浮点数的 EaseInQuart 缓动函数
    public static double EaseInQuart(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseInQuart 曲线的结果
        return c * (t /= d) * t * t * t + b;
    }

    // 实现一个双精度浮点数的 EaseOutQuart 缓动函数
    public static double EaseOutQuart(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseOutQuart 曲线的结果
        return -c * ((t = t / d - 1d) * t * t * t - 1d) + b;
    }

    // 实现一个双精度浮点数的 EaseInOutQuart 缓动函数
    public static double EaseInOutQuart(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 当时间在动画的一半时间内
        if ((t /= d / 2d) < 1d)
        {
            // 计算并返回 EaseInQuart 曲线的结果
            return c / 2d * t * t * t * t + b;
        }
        // 计算并返回 EaseOutQuart 曲线的结果
        return -c / 2d * ((t -= 2d) * t * t * t - 2d) + b;
    }

    // 实现一个双精度浮点数的 EaseInQuint 缓动函数
    public static double EaseInQuint(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseInQuint 曲线的结果
        return c * (t /= d) * t * t * t * t + b;
    }

    // 实现一个双精度浮点数的 EaseOutQuint 缓动函数
    public static double EaseOutQuint(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseOutQuint 曲线的结果
        return c * ((t = t / d - 1d) * t * t * t * t + 1d) + b;
    }

    // 实现一个双精度浮点数的 EaseInOutQuint 缓动函数
    public static double EaseInOutQuint(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 当时间在动画的一半时间内
        if ((t /= d / 2d) < 1d)
        {
            // 计算并返回 EaseInQuint 曲线的结果
            return c / 2d * t * t * t * t * t + b;
        }
        // 计算并返回 EaseOutQuint 曲线的结果
        return c / 2d * ((t -= 2d) * t * t * t * t + 2d) + b;
    }

    // 实现一个双精度浮点数的 EaseInSine 缓动函数
    public static double EaseInSine(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseInSine 曲线的结果
        return -c * Math.Cos(t / d * 1.5707963267948966d) + c + b;
    }

    // 实现一个双精度浮点数的 EaseOutSine 缓动函数
    public static double EaseOutSine(double t, double b, double c, double d)
    {
        c -= b;  // 计算变化量
        // 计算并返回 EaseOutSine 曲线的结果
        return c * Math.Sin(t / d * 1.5707963267948966d) + b;
    }

    // 实现一个双精度浮点数的 EaseInOutSine 缓动函数
    # 从c中减去b，计算一个基于三角函数的值并返回
    c -= b;
    # 计算最终值，基于余弦函数和时间参数
    return -c / 2d * (Math.Cos(3.141592653589793d * t / d) - 1d) + b;
    # 返回b加上c的结果，计算一个指数增长的动画效果
    public static double EaseInExpo(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 如果时间不为0，应用指数增长公式
        if (t != 0d)
        {
            return c * Math.Pow(2d, 10d * (t / d - 1d)) + b;
        }
        # 如果时间为0，返回起始值b
        return b;
    }
    # 返回b加上c的结果，计算一个指数衰减的动画效果
    public static double EaseOutExpo(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 如果时间不等于总时间，应用指数衰减公式
        if (t != d)
        {
            return c * (-Math.Pow(2d, -10d * t / d) + 1d) + b;
        }
        # 如果时间等于总时间，返回结束值b + c
        return b + c;
    }
    # 返回b加上c的结果，计算一个进出指数变化的动画效果
    public static double EaseInOutExpo(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 如果时间为0，返回起始值b
        if (t == 0d)
        {
            return b;
        }
        # 如果时间等于总时间，返回结束值b + c
        if (t == d)
        {
            return b + c;
        }
        # 如果时间小于总时间的一半，应用指数增长公式
        if ((t /= d / 2d) < 1d)
        {
            return c / 2d * Math.Pow(2d, 10d * (t - 1d)) + b;
        }
        # 否则，应用指数衰减公式
        return c / 2d * (-Math.Pow(2d, -10d * (t -= 1d)) + 2d) + b;
    }
    # 返回b加上c的结果，计算一个进圆形变化的动画效果
    public static double EaseInCirc(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 应用圆形变化公式
        return -c * (Math.Sqrt(1d - (t /= d) * t) - 1d) + b;
    }
    # 返回b加上c的结果，计算一个出圆形变化的动画效果
    public static double EaseOutCirc(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 应用圆形变化公式
        return c * Math.Sqrt(1d - (t = t / d - 1d) * t) + b;
    }
    # 返回b加上c的结果，计算一个进出圆形变化的动画效果
    public static double EaseInOutCirc(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 如果时间小于总时间的一半，应用进圆形变化公式
        if ((t /= d / 2d) < 1d)
        {
            return -c / 2d * (Math.Sqrt(1d - t * t) - 1d) + b;
        }
        # 否则，应用出圆形变化公式
        return c / 2d * (Math.Sqrt(1d - (t -= 2d) * t) + 1d) + b;
    }
    # 返回b加上c的结果，计算一个进弹性变化的动画效果
    public static double EaseInElastic(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 初始化弹性变化参数
        double num = 0d;
        double num2 = c;
        # 如果时间为0，返回起始值b
        if (t == 0d)
        {
            return b;
        }
        # 如果时间等于总时间，返回结束值b + c
        if ((t /= d) == 1d)
        {
            return b + c;
        }
        # 如果num为0，设置默认值
        if (num == 0d)
        {
            num = d * 0.3d;
        }
        double num3;
        # 如果c的绝对值小于num2，设置num3为num的四分之一
        if (num2 < Math.Abs(c))
        {
            num2 = c;
            num3 = num / 4d;
        }
        else
        {
            # 否则，计算num3为弹性振动周期的一部分
            num3 = num / 6.283185307179586d * Math.Asin(c / num2);
        }
        # 应用弹性变化公式
        return -(num2 * Math.Pow(2d, 10d * (t -= 1d)) * Math.Sin((t * d - num3) * 6.283185307179586d / num)) + b;
    }
    # 返回b加上c的结果，计算一个出弹性变化的动画效果
    public static double EaseOutElastic(double t, double b, double c, double d)
    {
        # 从c中减去b
        c -= b;
        # 初始化弹性变化参数
        double num = 0d;
        double num2 = c;
        # 如果时间为0，返回起始值b
        if (t == 0d)
        {
            return b;
        }
        # 如果时间等于总时间，返回结束值b + c
        if ((t /= d) == 1d)
        {
            return b + c;
        }
        # 如果num为0，设置默认值
        if (num == 0d)
        {
            num = d * 0.3d;
        }
        double num3;
        # 如果c的绝对值小于num2，设置num3为num的四分之一
        if (num2 < Math.Abs(c))
        {
            num2 = c;
            num3 = num / 4d;
        }
        else
        {
            # 否则，计算num3为弹性振动周期的一部分
            num3 = num / 6.283185307179586d * Math.Asin(c / num2);
        }
        # 应用弹性衰减公式
        return num2 * Math.Pow(2d, -10d * t) * Math.Sin((t * d - num3) * 6.283185307179586d / num) + c + b;
    }

    # 定义一个缓动函数，使用弹性函数的进出效果
    public static double EaseInOutElastic(double t, double b, double c, double d)
    {
        # 计算变动范围
        c -= b;
        double num = 0d;
        double num2 = c;
        # 起始时间点直接返回初始值
        if (t == 0d)
        {
            return b;
        }
        # 结束时间点直接返回结束值
        if ((t /= d / 2d) == 2d)
        {
            return b + c;
        }
        # 设置弹性系数
        if (num == 0d)
        {
            num = d * 0.44999999999999996d;
        }
        double num3;
        # 计算弹性参数
        if (num2 < Math.Abs(c))
        {
            num2 = c;
            num3 = num / 4d;
        }
        else
        {
            num3 = num / 6.283185307179586d * Math.Asin(c / num2);
        }
        # 处理前半段缓动
        if (t < 1d)
        {
            return -0.5d * (num2 * Math.Pow(2d, 10d * (t -= 1d)) * Math.Sin((t * d - num3) * 6.283185307179586d / num)) + b;
        }
        # 处理后半段缓动
        return num2 * Math.Pow(2d, -10d * (t -= 1d)) * Math.Sin((t * d - num3) * 6.283185307179586d / num) * 0.5d + c + b;
    }

    # 定义一个缓动函数，使用弹跳效果的进阶段
    public static double EaseInBounce(double t, double b, double c, double d)
    {
        c -= b;
        # 调用 EaseOutBounce 函数计算反向弹跳效果
        return c - EaseOutBounce(d - t, 0d, c, d) + b;
    }

    # 定义一个缓动函数，使用弹跳效果的出阶段
    public static double EaseOutBounce(double t, double b, double c, double d)
    {
        c -= b;
        # 按时间区间分别应用不同的弹跳公式
        if ((t /= d) < 0.36363636363636365d)
        {
            return c * (7.5625d * t * t) + b;
        }
        if (t < 0.7272727272727273d)
        {
            return c * (7.5625d * (t -= 0.5454545454545454d) * t + 0.75d) + b;
        }
        if (t < 0.9090909090909091d)
        {
            return c * (7.5625d * (t -= 0.8181818181818182d) * t + 0.9375d) + b;
        }
        return c * (7.5625d * (t -= 0.9545454545454546d) * t + 0.984375d) + b;
    }

    # 定义一个缓动函数，使用弹跳效果的进出阶段
    public static double EaseInOutBounce(double t, double b, double c, double d)
    {
        c -= b;
        # 前半段使用 EaseInBounce，后半段使用 EaseOutBounce
        if (t < d / 2d)
        {
            return EaseInBounce(t * 2d, 0d, c, d) * 0.5d + b;
        }
        return EaseOutBounce(t * 2d - d, 0d, c, d) * 0.5d + c * 0.5d + b;
    }

    # 定义一个线性缓动函数
    public static double Linear(double t, double b, double c, double d)
    {
        c -= b;
        # 计算线性变化
        return t / d * c + b;
    }

    # 定义一个缓动函数数组，包含所有预定义的缓动函数
    public static DoubleEasingAnimation[] Function { get; } = new DoubleEasingAnimation[]
    {
        EaseInOutQuad,
        EaseInQuad,
        EaseOutQuad,
        EaseInCubic,
        EaseOutCubic,
        EaseInOutCubic,
        EaseInQuart,
        EaseOutQuart,
        EaseInOutQuart,
        EaseInQuint,
        EaseOutQuint,
        EaseInOutQuint,
        EaseInSine,
        EaseOutSine,
        EaseInOutSine,
        EaseInExpo,
        EaseOutExpo,
        EaseInOutExpo,
        EaseInCirc,
        EaseOutCirc,
        EaseInOutCirc,
        EaseInElastic,
        EaseOutElastic,
        EaseInOutElastic,
        EaseInBounce,
        EaseOutBounce,
        EaseInOutBounce,
        Linear,
    };
} // 结束前一个代码块的闭合括号

public enum DoubleEasingAnimationType // 定义一个枚举类型 DoubleEasingAnimationType
{
    InOutQuad, // 插值动画：开始和结束都采用二次方函数
    InQuad, // 插值动画：开始时采用二次方函数
    OutQuad, // 插值动画：结束时采用二次方函数
    InCubic, // 插值动画：开始时采用三次方函数
    OutCubic, // 插值动画：结束时采用三次方函数
    InOutCubic, // 插值动画：开始和结束都采用三次方函数
    InQuart, // 插值动画：开始时采用四次方函数
    OutQuart, // 插值动画：结束时采用四次方函数
    InOutQuart, // 插值动画：开始和结束都采用四次方函数
    InQuint, // 插值动画：开始时采用五次方函数
    OutQuint, // 插值动画：结束时采用五次方函数
    InOutQuint, // 插值动画：开始和结束都采用五次方函数
    InSine, // 插值动画：开始时采用正弦函数
    OutSine, // 插值动画：结束时采用正弦函数
    InOutSine, // 插值动画：开始和结束都采用正弦函数
    InExpo, // 插值动画：开始时采用指数函数
    OutExpo, // 插值动画：结束时采用指数函数
    InOutExpo, // 插值动画：开始和结束都采用指数函数
    InCirc, // 插值动画：开始时采用圆形函数
    OutCirc, // 插值动画：结束时采用圆形函数
    InOutCirc, // 插值动画：开始和结束都采用圆形函数
    InElastic, // 插值动画：开始时采用弹性函数
    OutElastic, // 插值动画：结束时采用弹性函数
    InOutElastic, // 插值动画：开始和结束都采用弹性函数
    InBounce, // 插值动画：开始时采用弹跳函数
    OutBounce, // 插值动画：结束时采用弹跳函数
    InOutBounce, // 插值动画：开始和结束都采用弹跳函数
    Linear, // 插值动画：线性变化
}
```