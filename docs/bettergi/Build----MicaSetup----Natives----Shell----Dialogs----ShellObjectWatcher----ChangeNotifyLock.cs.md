# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ShellObjectWatcher\ChangeNotifyLock.cs`

```cs
# 命名空间定义和禁用特定警告
using System;
using System.Diagnostics;

namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618

# 内部类定义 ChangeNotifyLock
internal class ChangeNotifyLock
{
    # 定义只读字段 _event，用于存储事件标识符
    private readonly uint _event = 0;

    # 构造函数，接收一个 Message 对象
    internal ChangeNotifyLock(Message message)
    {
        # 调用 Shell32 的 SHChangeNotification_Lock 方法，锁定通知并获取事件信息
        var lockId = Shell32.SHChangeNotification_Lock(
                message.WParam, (int)message.LParam, out var pidl, out _event);
        try
        {
            # 记录事件信息到跟踪日志
            Trace.TraceInformation("Message: {0}", (ShellObjectChangeTypes)_event);

            # 将 pidl 转换为 ShellNotifyStruct 结构体
            var notifyStruct = pidl.MarshalAs<ShellNotifyStruct>();

            # 创建一个 GUID 对象，用于指定 IShellItem2 接口
            var guid = new Guid(ShellIIDGuid.IShellItem2);
            # 如果 notifyStruct.item1 不为 0 且事件类型不包含 SystemImageUpdate
            if (notifyStruct.item1 != 0 &&
                (((ShellObjectChangeTypes)_event) & ShellObjectChangeTypes.SystemImageUpdate) == ShellObjectChangeTypes.None)
            {
                # 如果成功创建 IShellItem2 对象
                if (CoreErrorHelper.Succeeded(Shell32.SHCreateItemFromIDList(
                    notifyStruct.item1, ref guid, out var nativeShellItem)))
                {
                    # 获取文件系统路径并赋值给 ItemName
                    nativeShellItem.GetDisplayName(ShellItemDesignNameOptions.FileSystemPath,
                        out var name);
                    ItemName = name;

                    # 记录 Item1 的名称到跟踪日志
                    Trace.TraceInformation("Item1: {0}", ItemName);
                }
            }
            else
            {
                # 如果不满足条件，将 ImageIndex 设为 notifyStruct.item1
                ImageIndex = (int)notifyStruct.item1;
            }

            # 如果 notifyStruct.item2 不为 0
            if (notifyStruct.item2 != 0)
            {
                # 如果成功创建 IShellItem2 对象
                if (CoreErrorHelper.Succeeded(Shell32.SHCreateItemFromIDList(
                    notifyStruct.item2, ref guid, out var nativeShellItem)))
                {
                    # 获取文件系统路径并赋值给 ItemName2
                    nativeShellItem.GetDisplayName(ShellItemDesignNameOptions.FileSystemPath,
                        out var name);
                    ItemName2 = name;

                    # 记录 Item2 的名称到跟踪日志
                    Trace.TraceInformation("Item2: {0}", ItemName2);
                }
            }
        }
        finally
        {
            # 确保在结束时解锁通知
            if (lockId != 0)
            {
                Shell32.SHChangeNotification_Unlock(lockId);
            }
        }
    }

    # 属性：判断事件是否由系统中断引发
    public bool FromSystemInterrupt => ((ShellObjectChangeTypes)_event & ShellObjectChangeTypes.FromInterrupt)
                != ShellObjectChangeTypes.None;

    # 属性：存储图像索引
    public int ImageIndex { get; private set; }
    # 属性：存储第一个项的名称
    public string ItemName { get; private set; }
    # 属性：存储第二个项的名称
    public string ItemName2 { get; private set; }

    # 属性：获取事件类型
    public ShellObjectChangeTypes ChangeType => (ShellObjectChangeTypes)_event;
}
```