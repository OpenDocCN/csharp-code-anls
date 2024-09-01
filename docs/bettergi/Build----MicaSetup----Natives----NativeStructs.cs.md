# `.\better-genshin-impact\Build\MicaSetup\Natives\NativeStructs.cs`

```cs
# 引入系统命名空间
﻿using System;
# 引入 COM 互操作命名空间
using System.Runtime.InteropServices;

# 定义 MicaSetup.Natives 命名空间
namespace MicaSetup.Natives;

# 使用布局控制特性，指定结构体的字段顺序，且使其可序列化和 COM 可见
[StructLayout(LayoutKind.Sequential), Serializable]
[ComVisible(true)]
# 定义 POINT 结构体，实现 IEquatable<POINT> 接口
public struct POINT : IEquatable<POINT>
{
    # 结构体字段：X 坐标
    public int X;
    # 结构体字段：Y 坐标
    public int Y;

    # 构造函数：初始化 POINT 结构体
    public POINT(int X, int Y)
    {
        this.X = X;
        this.Y = Y;
    }

    # X 属性：获取或设置 X 坐标
    public int x { get => x; set => x = value; }
    # Y 属性：获取或设置 Y 坐标
    public int y { get => y; set => y = value; }

    # 移动 POINT 结构体的位置：通过指定的 dx 和 dy 值
    public void Offset(int dx, int dy)
    {
        X += dx;
        Y += dy;
    }

    # 重载 == 操作符：比较两个 POINT 结构体是否相等
    public static bool operator ==(POINT first, POINT second) => first.X == second.X
        && first.Y == second.Y;

    # 重载 != 操作符：比较两个 POINT 结构体是否不相等
    public static bool operator !=(POINT first, POINT second) => !(first == second);

    # 重写 Equals 方法：比较对象是否等于当前 POINT 结构体
    public override bool Equals(object obj) => obj != null && obj is POINT p && this == p;

    # 实现 IEquatable<POINT> 接口的 Equals 方法：比较另一个 POINT 是否与当前 POINT 相等
    public bool Equals(POINT other) => other.X == X && other.Y == Y;

    # 重写 GetHashCode 方法：生成当前 POINT 结构体的哈希码
    public override int GetHashCode() => unchecked(X ^ Y);

    # 重写 ToString 方法：返回 POINT 结构体的字符串表示
    public override string ToString() => $"{{X={X},Y={Y}}}";
}

# 使用布局控制特性，指定结构体的字段顺序
[StructLayout(LayoutKind.Sequential)]
# 定义 RECT 结构体，实现 IEquatable<RECT> 接口
public struct RECT : IEquatable<RECT>
{
    # 结构体字段：矩形左边界
    public int left;
    # 结构体字段：矩形上边界
    public int top;
    # 结构体字段：矩形右边界
    public int right;
    # 结构体字段：矩形下边界
    public int bottom;

    # 构造函数：初始化 RECT 结构体
    public RECT(int left, int top, int right, int bottom)
    {
        this.left = left;
        this.top = top;
        this.right = right;
        this.bottom = bottom;
    }

    # Left 属性：获取或设置矩形的左边界
    public int Left { get => left; set => left = value; }
    # Right 属性：获取或设置矩形的右边界
    public int Right { get => right; set => right = value; }
    # Top 属性：获取或设置矩形的上边界
    public int Top { get => top; set => top = value; }
    # Bottom 属性：获取或设置矩形的下边界
    public int Bottom { get => bottom; set => bottom = value; }

    # X 属性：获取矩形的左边界，设置时调整右边界
    public int X
    {
        get => left;
        set
        {
            right -= left - value;
            left = value;
        }
    }

    # Y 属性：获取矩形的上边界，设置时调整下边界
    public int Y
    {
        get => top;
        set
        {
            bottom -= top - value;
            top = value;
        }
    }

    # Height 属性：获取矩形的高度，设置时调整下边界
    public int Height
    {
        get => bottom - top;
        set => bottom = value + top;
    }

    # Width 属性：获取矩形的宽度，设置时调整右边界
    public int Width
    {
        get => right - left;
        set => right = value + left;
    }

    # Location 属性：获取矩形的左上角位置，设置时调整 X 和 Y
    public POINT Location
    {
        get => new(left, top);
        set
        {
            X = value.X;
            Y = value.Y;
        }
    }

    # Size 属性：获取矩形的大小，设置时调整宽度和高度
    public SIZE Size
    {
        get => new(Width, Height);
        set
        {
            Width = value.Width;
            Height = value.Height;
        }
    }

    # 判断矩形是否为空（所有边界均为 0）
    public bool IsEmpty => left == 0 && top == 0 && right == 0 && bottom == 0;

    # 重载 == 操作符：比较两个 RECT 结构体是否相等
    public static bool operator ==(RECT first, RECT second) => first.Left == second.Left
            && first.Top == second.Top
            && first.Right == second.Right
            && first.Bottom == second.Bottom;

    # 重载 != 操作符：比较两个 RECT 结构体是否不相等
    public static bool operator !=(RECT first, RECT second) => !(first == second);

    # 定义一个静态只读的 Empty RECT 对象
    public static readonly RECT Empty = new();

    # 重写 Equals 方法：比较对象是否等于当前 RECT 结构体
    public override bool Equals(object obj) => obj != null && obj is RECT rect && this == rect;
    // 比较当前对象和传入的 RECT 对象是否具有相同的边界
    public bool Equals(RECT r) => r.left == left && r.top == top && r.right == right && r.bottom == bottom;

    // 返回当前 RECT 对象的字符串表示，包含所有边界值
    public override string ToString() => $"{{left={left},top={top},right={right},bottom={bottom}}}";

    // 计算当前 RECT 对象的哈希码，结合所有边界值生成唯一哈希
    public override int GetHashCode()
    {
        // 计算左边界的哈希码
        var hash = Left.GetHashCode();
        // 结合顶边界的哈希码
        hash = hash * 31 + Top.GetHashCode();
        // 结合右边界的哈希码
        hash = hash * 31 + Right.GetHashCode();
        // 结合底边界的哈希码
        hash = hash * 31 + Bottom.GetHashCode();
        // 返回最终的哈希码
        return hash;
    }

    // 从另一个 RECT 对象创建一个新的 RECT 对象
    public static RECT FromRECT(RECT r) => new(r.left, r.top, r.right, r.bottom);
}

# 指定结构体的布局顺序为顺序布局，并标记为可序列化
[StructLayout(LayoutKind.Sequential), Serializable]
public struct SIZE : IEquatable<SIZE>
{
    // 宽度字段
    private int width;
    // 高度字段
    private int height;

    // 默认构造函数
    public SIZE()
    {
    }

    // 带参数的构造函数，初始化宽度和高度
    public SIZE(int width, int height)
    {
        this.width = width;
        this.height = height;
    }

    // 高度属性的 getter 和 setter
    public int Height { get => height; set => height = value; }
    // 判断结构体是否为空（宽度和高度都为0）
    public bool IsEmpty => width == 0 && height == 0;
    // 宽度属性的 getter 和 setter
    public int Width { get => width; set => width = value; }

    // 实现 IEquatable 接口的 Equals 方法，比较宽度或高度是否相等
    public bool Equals(SIZE other) => width == other.width || height == other.height;

    // 重写 GetHashCode 方法，如果为空返回0，否则返回宽度和高度的哈希码异或值
    public override int GetHashCode() => IsEmpty ? 0 : width.GetHashCode() ^ height.GetHashCode();

    // 重写 ToString 方法，返回格式化字符串表示
    public override string ToString() => $"{{cx={width}, cy={height}}}";

    // 静态 readonly 只读字段，表示空的 SIZE 实例
    public static readonly SIZE Empty = new();
}

# 指定结构体的布局顺序为顺序布局
[StructLayout(LayoutKind.Sequential)]
public struct OSVERSIONINFOEX
{
    // 操作系统版本信息结构体的大小
    public int OSVersionInfoSize;
    // 主版本号
    public int MajorVersion;
    // 次版本号
    public int MinorVersion;
    // 构建号
    public int BuildNumber;
    // 平台标识符
    public int PlatformId;

    // 操作系统的服务包版本，长度为128的字符串
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 128)]
    public string CSDVersion;

    // 服务包主要版本号
    public ushort ServicePackMajor;
    // 服务包次要版本号
    public ushort ServicePackMinor;
    // 套件掩码
    public short SuiteMask;
    // 产品类型
    public byte ProductType;
    // 保留字段
    public byte Reserved;
}

public struct HRESULT
{
    // 常量表示成功的结果
    public const int S_OK = 0;
}
```