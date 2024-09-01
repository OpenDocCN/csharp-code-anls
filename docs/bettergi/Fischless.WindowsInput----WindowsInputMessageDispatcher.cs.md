# `.\better-genshin-impact\Fischless.WindowsInput\WindowsInputMessageDispatcher.cs`

```cs
# 引用系统运行时库的互操作服务
﻿using System.Runtime.InteropServices;
# 引用 Vanara.PInvoke 库中的 PInvoke 功能
using Vanara.PInvoke;

# 命名空间定义
namespace Fischless.WindowsInput;

# 内部类实现 IInputMessageDispatcher 接口
internal class WindowsInputMessageDispatcher : IInputMessageDispatcher
{
    # 方法：分发输入
    public void DispatchInput(User32.INPUT[] inputs)
    {
        # 如果输入数组为 null，抛出 ArgumentNullException 异常
        if (inputs == null)
        {
            throw new ArgumentNullException(nameof(inputs));
        }

        # 如果输入数组长度为 0，抛出 ArgumentException 异常
        if (inputs.Length == 0)
        {
            throw new ArgumentException("The input array was empty", nameof(inputs));
        }

        # 调用 SendInput 方法，发送输入数据
        uint num = User32.SendInput((uint)inputs.Length, inputs, Marshal.SizeOf(typeof(User32.INPUT)));

        # 如果发送的输入数量与数组长度不匹配，抛出异常
        if (num != (ulong)(long)inputs.Length)
        {
            throw new Exception("Some simulated input commands were not sent successfully. The most common reason for this happening are the security features of Windows including User Interface Privacy Isolation (UIPI). Your application can only send commands to applications of the same or lower elevation. Similarly certain commands are restricted to Accessibility/UIAutomation applications. Refer to the project home page and the code samples for more information.");
        }
    }
}
```