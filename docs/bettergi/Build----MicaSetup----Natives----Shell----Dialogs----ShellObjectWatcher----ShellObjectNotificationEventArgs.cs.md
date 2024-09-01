# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ShellObjectWatcher\ShellObjectNotificationEventArgs.cs`

```cs
# 定义 ShellObjectNotificationEventArgs 类，继承自 EventArgs
public class ShellObjectNotificationEventArgs : EventArgs
{
    # 定义一个只读属性 ChangeType，表示变更类型
    public ShellObjectChangeTypes ChangeType { get; private set; }

    # 定义一个只读属性 FromSystemInterrupt，表示是否由系统中断触发
    public bool FromSystemInterrupt { get; private set; }

    # 内部构造函数，初始化 ChangeType 和 FromSystemInterrupt 属性
    internal ShellObjectNotificationEventArgs(ChangeNotifyLock notifyLock)
    {
        # 从 ChangeNotifyLock 对象中获取变更类型
        ChangeType = notifyLock.ChangeType;
        # 从 ChangeNotifyLock 对象中获取是否由系统中断触发
        FromSystemInterrupt = notifyLock.FromSystemInterrupt;
    }
}

# 定义 ShellObjectChangedEventArgs 类，继承自 ShellObjectNotificationEventArgs
public class ShellObjectChangedEventArgs : ShellObjectNotificationEventArgs
{
    # 定义一个只读属性 Path，表示对象的路径
    public string Path { get; private set; }

    # 内部构造函数，初始化 Path 属性，并调用基类构造函数
    internal ShellObjectChangedEventArgs(ChangeNotifyLock notifyLock)
        : base(notifyLock) => Path = notifyLock.ItemName;
}

# 定义 ShellObjectRenamedEventArgs 类，继承自 ShellObjectChangedEventArgs
public class ShellObjectRenamedEventArgs : ShellObjectChangedEventArgs
{
    # 定义一个只读属性 NewPath，表示新路径
    public string NewPath { get; private set; }

    # 内部构造函数，初始化 NewPath 属性，并调用基类构造函数
    internal ShellObjectRenamedEventArgs(ChangeNotifyLock notifyLock)
        : base(notifyLock) => NewPath = notifyLock.ItemName2;
}

# 定义 SystemImageUpdatedEventArgs 类，继承自 ShellObjectNotificationEventArgs
public class SystemImageUpdatedEventArgs : ShellObjectNotificationEventArgs
{
    # 定义一个只读属性 ImageIndex，表示图像索引
    public int ImageIndex { get; private set; }

    # 内部构造函数，初始化 ImageIndex 属性，并调用基类构造函数
    internal SystemImageUpdatedEventArgs(ChangeNotifyLock notifyLock)
        : base(notifyLock) => ImageIndex = notifyLock.ImageIndex;
}
```