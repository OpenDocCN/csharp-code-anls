# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Window\SnapLayout.cs`

```cs
﻿using MicaSetup.Helper;  // 引用 MicaSetup.Helper 命名空间
using MicaSetup.Natives;  // 引用 MicaSetup.Natives 命名空间
using Microsoft.Win32;  // 引用 Microsoft.Win32 命名空间
using System;  // 引用 System 命名空间
using System.Diagnostics;  // 引用 System.Diagnostics 命名空间
using System.Windows;  // 引用 System.Windows 命名空间
using System.Windows.Automation.Peers;  // 引用 System.Windows.Automation.Peers 命名空间
using System.Windows.Automation.Provider;  // 引用 System.Windows.Automation.Provider 命名空间
using System.Windows.Controls;  // 引用 System.Windows.Controls 命名空间
using System.Windows.Media;  // 引用 System.Windows.Media 命名空间
using Point = System.Windows.Point;  // 使用 System.Windows.Point 作为 Point
using Size = System.Windows.Size;  // 使用 System.Windows.Size 作为 Size

namespace MicaSetup.Design.Controls;  // 定义 MicaSetup.Design.Controls 命名空间

/// <summary>
/// https://learn.microsoft.com/zh-cn/windows/apps/desktop/modernize/apply-snap-layout-menu  // 文档链接说明
/// </summary>
public sealed class SnapLayout  // 定义封闭类 SnapLayout
{
    public bool IsRegistered { get; private set; } = false;  // 属性，表示是否已注册
    public bool IsSupported => Environment.OSVersion.Platform == PlatformID.Win32NT && Environment.OSVersion.Version.Build > 20000;  // 属性，检查当前操作系统是否支持
    public static bool IsEnabled { get; } = IsSnapLayoutEnabled();  // 静态属性，检查 SnapLayout 是否启用

    private Button? button;  // 私有字段，存储按钮实例
    private bool isButtonFocused;  // 私有字段，表示按钮是否被聚焦

    private Window? window;  // 私有字段，存储窗口实例

    public void Register(Button button)  // 公共方法，注册按钮
    {
        isButtonFocused = false;  // 重置按钮焦点状态
        this.button = button;  // 保存按钮实例
        IsRegistered = true;  // 标记为已注册
    }

    public nint WndProc(nint hWnd, int msg, nint wParam, nint lParam, ref bool handled)  // 处理窗口消息的公共方法
    {
        if (!IsRegistered) return 0;  // 如果未注册，返回 0
        switch ((User32.WindowMessage)msg)  // 根据消息类型进行处理
        {
            case User32.WindowMessage.WM_NCLBUTTONDOWN:  // 处理 WM_NCLBUTTONDOWN 消息
                if (IsOverButton(lParam))  // 如果鼠标点击在按钮上
                {
                    window ??= Window.GetWindow(button);  // 如果窗口为空，则获取关联窗口
                    if (window != null)  // 如果窗口存在
                    {
                        WindowSystemCommands.MaximizeOrRestoreWindow(window);  // 最大化或恢复窗口
                    }
                    else  // 如果窗口不存在
                    {
                        if (new ButtonAutomationPeer(button).GetPattern(PatternInterface.Invoke) is IInvokeProvider invokeProv)  // 获取按钮的自动化模式
                        {
                            invokeProv?.Invoke();  // 调用按钮的操作
                        }
                    }
                    handled = true;  // 标记消息已处理
                }
                break;

            case User32.WindowMessage.WM_NCMOUSELEAVE:  // 处理 WM_NCMOUSELEAVE 消息
                DefocusButton();  // 取消按钮焦点
                break;

            case User32.WindowMessage.WM_NCHITTEST:  // 处理 WM_NCHITTEST 消息
                if (IsEnabled)  // 如果启用 SnapLayout
                {
                    if (IsOverButton(lParam))  // 如果鼠标在按钮上
                    {
                        FocusButton();  // 聚焦按钮
                        handled = true;  // 标记消息已处理
                    }
                    else  // 如果鼠标不在按钮上
                    {
                        DefocusButton();  // 取消按钮焦点
                    }
                }
                return (int)User32.HitTestValues.HTMAXBUTTON;  // 返回最大化按钮的测试值

            case User32.WindowMessage.WM_SETCURSOR:  // 处理 WM_SETCURSOR 消息
                if (isButtonFocused)  // 如果按钮被聚焦
                {
                    handled = true;  // 标记消息已处理
                }
                break;

            default:  // 处理其他消息
                handled = false;  // 标记消息未处理
                break;
        }
        return (int)User32.HitTestValues.HTCLIENT;  // 返回客户端区域的测试值
    }

    private void FocusButton()  // 私有方法，聚焦按钮
    {
        # 如果按钮已经被聚焦，则直接返回，不做任何操作
        if (isButtonFocused) return;

        # 如果按钮不为 null，则更新其样式属性
        if (button != null)
        {
            # 设置按钮的背景为鼠标悬停时的样式
            button.Background = (Brush)Application.Current.FindResource("ButtonBackgroundPointerOver");
            # 设置按钮的边框为鼠标悬停时的样式
            button.BorderBrush = (Brush)Application.Current.FindResource("ButtonBorderBrushPointerOver");
            # 设置按钮的前景色为鼠标悬停时的样式
            button.Foreground = (Brush)Application.Current.FindResource("ButtonForegroundPointerOver");
        }
        # 标记按钮已被聚焦
        isButtonFocused = true;
    }

    private void DefocusButton()
    {
        # 如果按钮未被聚焦，则直接返回，不做任何操作
        if (!isButtonFocused) return;

        # 清除按钮的背景样式
        button?.ClearValue(Control.BackgroundProperty);
        # 清除按钮的边框样式
        button?.ClearValue(Control.BorderBrushProperty);
        # 清除按钮的前景色样式
        button?.ClearValue(Control.ForegroundProperty);
        # 标记按钮未被聚焦
        isButtonFocused = false;
    }

    private bool IsOverButton(nint lParam)
    {
        # 如果按钮为 null，则返回 false，表示鼠标不在按钮上
        if (button == null)
        {
            return false;
        }

        try
        {
            # 从 lParam 获取鼠标的 X 坐标
            int x = Macro.GET_X_LPARAM(lParam);
            # 从 lParam 获取鼠标的 Y 坐标
            int y = Macro.GET_Y_LPARAM(lParam);

            # 获取按钮在屏幕上的矩形区域
            Rect rect = new(button.PointToScreen(new Point()), new Size(DpiHelper.CalcDPIX(button.ActualWidth), DpiHelper.CalcDPIY(button.ActualHeight)));

            # 判断鼠标坐标是否在按钮的矩形区域内
            if (rect.Contains(new Point(x, y)))
            {
                return true;
            }
        }
        catch (OverflowException)
        {
            # 如果发生溢出异常，认为鼠标在按钮上
            return true;
        }
        # 如果鼠标不在按钮上，返回 false
        return false;
    }

    private static bool IsSnapLayoutEnabled()
    {
        try
        {
            # 尝试打开注册表键，检查是否启用了 Snap 布局
            using RegistryKey? key = Registry.CurrentUser.OpenSubKey(@"Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced", true);
            object? registryValueObject = key?.GetValue("EnableSnapAssistFlyout");

            # 如果注册表值为 null，则默认认为启用了 Snap 布局
            if (registryValueObject == null)
            {
                return true;
            }
            # 将注册表值转换为整数并检查其是否大于 0
            int registryValue = (int)registryValueObject;
            return registryValue > 0;
        }
        catch (Exception e)
        {
            # 如果发生异常，将错误信息写入调试输出
            Debug.WriteLine(e);
        }
        # 如果发生异常或无法获取值，默认认为启用了 Snap 布局
        return true;
    }
} 
// 结束之前的类或方法

file static class Macro
{
    // 定义一个静态类 Macro，用于提供几个静态方法

    // 定义一个静态方法 LOWORD，用于从 nint 值中提取低 16 位
    public static ushort LOWORD(nint value) => (ushort)((long)value & 0xFFFF);

    // 定义一个静态方法 HIWORD，用于从 nint 值中提取高 16 位
    public static ushort HIWORD(nint value) => (ushort)((((long)value) >> 0x10) & 0xFFFF);

    // 定义一个静态方法 GET_X_LPARAM，用于从 nint 值中提取 X 坐标（低 16 位）
    public static int GET_X_LPARAM(nint wParam) => LOWORD(wParam);

    // 定义一个静态方法 GET_Y_LPARAM，用于从 nint 值中提取 Y 坐标（高 16 位）
    public static int GET_Y_LPARAM(nint wParam) => HIWORD(wParam);
}
```