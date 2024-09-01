# `.\better-genshin-impact\BetterGenshinImpact\Core\Simulator\PostMessageSimulator.cs`

```cs
﻿using System;
using System.Threading;
using Vanara.PInvoke;

namespace BetterGenshinImpact.Core.Simulator;

/// <summary>
///     虚拟键代码
///     https://learn.microsoft.com/zh-cn/windows/win32/inputdev/virtual-key-codes
///     User32.VK.VK_SPACE 键盘空格键
/// </summary>
public class PostMessageSimulator
{
    public static readonly uint WM_LBUTTONDOWN = 0x201; //定义鼠标左键按下的消息代码

    public static readonly uint WM_LBUTTONUP = 0x202; //定义鼠标左键释放的消息代码

    public static readonly uint WM_RBUTTONDOWN = 0x204; //定义鼠标右键按下的消息代码
    public static readonly uint WM_RBUTTONUP = 0x205; //定义鼠标右键释放的消息代码

    private readonly IntPtr _hWnd; // 存储窗口句柄

    public PostMessageSimulator(IntPtr hWnd)
    {
        _hWnd = hWnd; // 初始化窗口句柄
    }

    /// <summary>
    ///     指定位置并按下左键
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    public PostMessageSimulator LeftButtonClick(int x, int y)
    {
        IntPtr p = (y << 16) | x; // 将 x 和 y 合并为一个 IntPtr 类型的坐标
        User32.PostMessage(_hWnd, WM_LBUTTONDOWN, IntPtr.Zero, p); // 发送鼠标左键按下的消息
        Thread.Sleep(100); // 等待 100 毫秒
        User32.PostMessage(_hWnd, WM_LBUTTONUP, IntPtr.Zero, p); // 发送鼠标左键释放的消息
        return this; // 返回当前对象以支持链式调用
    }

    /// <summary>
    ///     指定位置并按下左键
    /// </summary>
    /// <param name="x"></param>
    /// <param name="y"></param>
    public PostMessageSimulator LeftButtonClickBackground(int x, int y)
    {
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_ACTIVATE, 1, 0); // 激活窗口
        var p = MakeLParam(x, y); // 使用 MakeLParam 方法构造消息参数
        User32.PostMessage(_hWnd, WM_LBUTTONDOWN, 1, p); // 发送鼠标左键按下的消息
        Thread.Sleep(100); // 等待 100 毫秒
        User32.PostMessage(_hWnd, WM_LBUTTONUP, 0, p); // 发送鼠标左键释放的消息
        return this; // 返回当前对象以支持链式调用
    }

    public static int MakeLParam(int x, int y) => (y << 16) | (x & 0xFFFF); // 将 x 和 y 组合成 LParam 格式的参数

    public PostMessageSimulator LeftButtonClick()
    {
        IntPtr p = (16 << 16) | 16; // 使用默认位置 (16, 16) 构造坐标
        User32.PostMessage(_hWnd, WM_LBUTTONDOWN, IntPtr.Zero, p); // 发送鼠标左键按下的消息
        Thread.Sleep(100); // 等待 100 毫秒
        User32.PostMessage(_hWnd, WM_LBUTTONUP, IntPtr.Zero, p); // 发送鼠标左键释放的消息
        return this; // 返回当前对象以支持链式调用
    }

    public PostMessageSimulator LeftButtonClickBackground()
    {
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_ACTIVATE, 1, 0); // 激活窗口
        IntPtr p = (16 << 16) | 16; // 使用默认位置 (16, 16) 构造坐标
        User32.PostMessage(_hWnd, WM_LBUTTONDOWN, IntPtr.Zero, p); // 发送鼠标左键按下的消息
        Thread.Sleep(100); // 等待 100 毫秒
        User32.PostMessage(_hWnd, WM_LBUTTONUP, IntPtr.Zero, p); // 发送鼠标左键释放的消息
        return this; // 返回当前对象以支持链式调用
    }

    /// <summary>
    ///     默认位置左键按下
    /// </summary>
    public PostMessageSimulator LeftButtonDown()
    {
        User32.PostMessage(_hWnd, WM_LBUTTONDOWN, IntPtr.Zero); // 发送鼠标左键按下的消息
        return this; // 返回当前对象以支持链式调用
    }

    /// <summary>
    ///     默认位置左键释放
    /// </summary>
    public PostMessageSimulator LeftButtonUp()
    {
        User32.PostMessage(_hWnd, WM_LBUTTONUP, IntPtr.Zero); // 发送鼠标左键释放的消息
        return this; // 返回当前对象以支持链式调用
    }

    /// <summary>
    ///     默认位置右键按下
    /// </summary>
    public PostMessageSimulator RightButtonDown()
    {
        User32.PostMessage(_hWnd, WM_RBUTTONDOWN, IntPtr.Zero); // 发送鼠标右键按下的消息
        return this; // 返回当前对象以支持链式调用
    }

    /// <summary>
    ///     默认位置右键释放
    /// </summary>
    public PostMessageSimulator RightButtonUp()
    # 向窗口发送右键释放消息
    User32.PostMessage(_hWnd, WM_RBUTTONUP, IntPtr.Zero);
    # 返回当前实例
    return this;

    public PostMessageSimulator RightButtonClick()
    {
        # 设置鼠标右键按下的位掩码
        IntPtr p = (16 << 16) | 16;
        # 向窗口发送右键按下消息
        User32.PostMessage(_hWnd, WM_RBUTTONDOWN, IntPtr.Zero, p);
        # 等待 100 毫秒
        Thread.Sleep(100);
        # 向窗口发送右键释放消息
        User32.PostMessage(_hWnd, WM_RBUTTONUP, IntPtr.Zero, p);
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyPress(User32.VK vk)
    {
        # 向窗口发送键盘按下消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYDOWN, (nint)vk, 0x1e0001);
        # 向窗口发送字符输入消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_CHAR, (nint)vk, 0x1e0001);
        # 向窗口发送键盘释放消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYUP, (nint)vk, unchecked((nint)0xc01e0001));
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyPress(User32.VK vk, int ms)
    {
        # 向窗口发送键盘按下消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYDOWN, (nint)vk, 0x1e0001);
        # 等待指定毫秒数
        Thread.Sleep(ms);
        # 向窗口发送字符输入消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_CHAR, (nint)vk, 0x1e0001);
        # 向窗口发送键盘释放消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYUP, (nint)vk, unchecked((nint)0xc01e0001));
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator LongKeyPress(User32.VK vk)
    {
        # 向窗口发送键盘按下消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYDOWN, (nint)vk, 0x1e0001);
        # 等待 1000 毫秒
        Thread.Sleep(1000);
        # 向窗口发送字符输入消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_CHAR, (nint)vk, 0x1e0001);
        # 向窗口发送键盘释放消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYUP, (nint)vk, unchecked((nint)0xc01e0001));
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyDown(User32.VK vk)
    {
        # 向窗口发送键盘按下消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYDOWN, (nint)vk, 0x1e0001);
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyUp(User32.VK vk)
    {
        # 向窗口发送键盘释放消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYUP, (nint)vk, unchecked((nint)0xc01e0001));
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyPressBackground(User32.VK vk)
    {
        # 激活窗口
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_ACTIVATE, 1, 0);
        # 向窗口发送键盘按下消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYDOWN, (nint)vk, 0x1e0001);
        # 向窗口发送字符输入消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_CHAR, (nint)vk, 0x1e0001);
        # 向窗口发送键盘释放消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYUP, (nint)vk, unchecked((nint)0xc01e0001));
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyDownBackground(User32.VK vk)
    {
        # 激活窗口
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_ACTIVATE, 1, 0);
        # 向窗口发送键盘按下消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYDOWN, (nint)vk, 0x1e0001);
        # 返回当前实例
        return this;
    }

    public PostMessageSimulator KeyUpBackground(User32.VK vk)
    {
        # 激活窗口
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_ACTIVATE, 1, 0);
        # 向窗口发送键盘释放消息
        User32.PostMessage(_hWnd, User32.WindowMessage.WM_KEYUP, (nint)vk, unchecked((nint)0xc01e0001));
        # 返回当前实例
        return this;
    }
    // 定义一个公共方法 Sleep，接收一个整数参数 ms（毫秒）
    public PostMessageSimulator Sleep(int ms)
    {
        // 使当前线程休眠指定的毫秒数
        Thread.Sleep(ms);
        // 返回当前对象实例，以支持链式调用
        return this;
    }
# 结束一个代码块或结构体（例如类、函数等）
}
```