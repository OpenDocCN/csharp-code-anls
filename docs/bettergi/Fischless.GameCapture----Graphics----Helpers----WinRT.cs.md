# `.\better-genshin-impact\Fischless.GameCapture\Graphics\Helpers\WinRT.cs`

```cs
# 引用系统组件模型相关的命名空间
﻿using System.ComponentModel;
using System.Reflection;
using System.Runtime.InteropServices;
using WinRT;

# 定义用于图形捕获的命名空间
namespace Fischless.GameCapture.Graphics;

# 关闭特定的编译器警告
#pragma warning disable CS0649

# 定义图形捕获项的 COM 互操作接口
[ComImport]
[Guid("3628E81B-3CAC-4C60-B7F4-23CE0E0C3356")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IGraphicsCaptureItemInterop
{
    # 创建一个用于窗口的图形捕获项
    int CreateForWindow([In] nint window, [In] ref Guid iid, out nint result);

    # 创建一个用于监视器的图形捕获项
    int CreateForMonitor([In] nint monitor, [In] ref Guid iid, out nint result);
}

# 定义激活工厂的虚表结构
[Guid("00000035-0000-0000-C000-000000000046")]
internal unsafe struct IActivationFactoryVftbl
{
    # 激活工厂接口的虚表
    public readonly IInspectable.Vftbl IInspectableVftbl;
    # 指向激活实例方法的指针
    private readonly void* _ActivateInstance;

    # 激活实例的委托方法
    public delegate* unmanaged[Stdcall]<nint, nint*, int> ActivateInstance => (delegate* unmanaged[Stdcall]<nint, nint*, int>)_ActivateInstance;
}

internal class Platform
{
    # 从 COM 中减少 MTA 使用计数
    [DllImport("api-ms-win-core-com-l1-1-0.dll")]
    internal static extern int CoDecrementMTAUsage(nint cookie);

    # 从 COM 中增加 MTA 使用计数
    [DllImport("api-ms-win-core-com-l1-1-0.dll")]
    internal static extern unsafe int CoIncrementMTAUsage(nint* cookie);

    # 获取激活工厂
    [DllImport("api-ms-win-core-winrt-l1-1-0.dll")]
    internal static extern unsafe int RoGetActivationFactory(nint runtimeClassId, ref Guid iid, nint* factory);
}

/// <summary>
/// 参考链接：https://github.com/zlatanov/windows-screen-recorder
/// </summary>
internal static class WinrtModule
{
    # 激活工厂的缓存字典
    private static readonly Dictionary<string, ObjectReference<IActivationFactoryVftbl>> Cache = [];

    # 获取指定运行时类 ID 的激活工厂
    public static ObjectReference<IActivationFactoryVftbl> GetActivationFactory(string runtimeClassId)
    {
        lock (Cache)
        {
            # 如果缓存中已存在，直接返回
            if (Cache.TryGetValue(runtimeClassId, out var factory))
                return factory;

            # 创建运行时类 ID 的字符串封送处理器
            var m = MarshalString.CreateMarshaler(runtimeClassId);

            try
            {
                # 获取激活工厂实例指针
                var instancePtr = GetActivationFactory(MarshalString.GetAbi(m));

                # 将实例指针附加到对象引用并缓存
                factory = ObjectReference<IActivationFactoryVftbl>.Attach(ref instancePtr);
                Cache.Add(runtimeClassId, factory);

                return factory;
            }
            finally
            {
                # 释放字符串封送处理器
                m.Dispose();
            }
        }
    }

    # 获取激活工厂的内部方法
    private static unsafe nint GetActivationFactory(nint hstrRuntimeClassId)
    {
        # 检查和设置 MTA 使用计数
        if (s_cookie == IntPtr.Zero)
        {
            lock (s_lock)
            {
                if (s_cookie == IntPtr.Zero)
                {
                    nint cookie;
                    Marshal.ThrowExceptionForHR(Platform.CoIncrementMTAUsage(&cookie));

                    s_cookie = cookie;
                }
            }
        }

        # 获取激活工厂的 GUID 和实例指针
        Guid iid = typeof(IActivationFactoryVftbl).GUID;
        nint instancePtr;
        int hr = Platform.RoGetActivationFactory(hstrRuntimeClassId, ref iid, &instancePtr);

        # 根据结果返回或抛出异常
        if (hr == 0)
            return instancePtr;

        throw new Win32Exception(hr);
    }
    // 试图复活一个对象引用，如果对象已被释放则重新激活它
    public static bool ResurrectObjectReference(IObjectReference objRef)
    {
        // 获取对象内部名为 "disposed" 的私有字段
        var disposedField = objRef.GetType().GetField("disposed", BindingFlags.NonPublic | BindingFlags.Instance)!;
        // 检查对象是否已经被标记为已释放
        if (!(bool)disposedField.GetValue(objRef)!)
            return false;
        // 将 "disposed" 字段设置为 false，表示对象尚未释放
        disposedField.SetValue(objRef, false);
        // 重新注册对象以便垃圾回收器可以最终处理它
        GC.ReRegisterForFinalize(objRef);
        // 返回 true，表示对象成功复活
        return true;
    }

    // 静态字段，存储某种 cookie 信息
    private static nint s_cookie;
    // 静态字段，用于线程同步
    private static readonly object s_lock = new();
可以提供一下你需要注释的代码吗？这样我能更好地帮助你。
```