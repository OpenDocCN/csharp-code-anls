# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ShellObjectWatcher\ShellObjectWatcher.cs`

```cs
﻿using System;
using System.ComponentModel;
using System.Threading;

namespace MicaSetup.Shell.Dialogs;

// 定义一个可释放的 ShellObjectWatcher 类
public class ShellObjectWatcher : IDisposable
{
    // 壳对象实例
    private readonly ShellObject _shellObject;
    // 是否递归监视
    private readonly bool _recursive;

    // 变更通知事件管理器
    private readonly ChangeNotifyEventManager _manager = new();
    // 监听器句柄
    private readonly nint _listenerHandle;
    // 消息标识
    private readonly uint _message;

    // 注册 ID
    private uint _registrationId;
    // 运行状态
    private volatile bool _running;

    // 当前同步上下文
    private readonly SynchronizationContext _context = SynchronizationContext.Current;

    // 构造函数，初始化 ShellObjectWatcher 实例
    public ShellObjectWatcher(ShellObject shellObject, bool recursive)
    {
        // 如果当前同步上下文为空，则创建并设置新的同步上下文
        if (_context == null)
        {
            _context = new SynchronizationContext();
            SynchronizationContext.SetSynchronizationContext(_context);
        }

        // 初始化 ShellObject 和递归标志
        _shellObject = shellObject ?? throw new ArgumentNullException("shellObject");
        _recursive = recursive;

        // 注册窗口消息监听器
        var result = MessageListenerFilter.Register(OnWindowMessageReceived);
        _listenerHandle = result.WindowHandle;
        _message = result.Message;
    }

    // 获取或设置当前运行状态
    public bool Running
    {
        get => _running;
        private set => _running = value;
    }

    // 启动监视器
    public void Start()
    {
        // 如果已经在运行，则直接返回
        if (Running) { return; }

        // 设置变更通知条目
        var entry = new SHChangeNotifyEntry
        {
            recursively = _recursive,
            pIdl = _shellObject.PIDL
        };

        // 注册变更通知
        _registrationId = Shell32.SHChangeNotifyRegister(
            _listenerHandle,
            ShellChangeNotifyEventSource.ShellLevel | ShellChangeNotifyEventSource.InterruptLevel | ShellChangeNotifyEventSource.NewDelivery,
             _manager.RegisteredTypes,
            _message,
            1,
            ref entry);

        // 如果注册失败，则抛出异常
        if (_registrationId == 0)
        {
            throw new Win32Exception(LocalizedMessages.ShellObjectWatcherRegisterFailed);
        }

        // 更新运行状态
        Running = true;
    }

    // 停止监视器
    public void Stop()
    {
        // 如果未运行，则直接返回
        if (!Running) { return; }
        // 注销变更通知
        if (_registrationId > 0)
        {
            Shell32.SHChangeNotifyDeregister(_registrationId);
            _registrationId = 0;
        }
        // 更新运行状态
        Running = false;
    }

    // 处理窗口消息接收事件
    private void OnWindowMessageReceived(WindowMessageEventArgs e)
    {
        // 如果消息匹配预期消息，则处理变更通知事件
        if (e.Message.Msg == _message)
        {
            _context.Send(x => ProcessChangeNotificationEvent(e), null);
        }
    }

    // 抛出异常如果当前正在运行
    private void ThrowIfRunning()
    {
        if (Running)
        {
            throw new InvalidOperationException(LocalizedMessages.ShellObjectWatcherUnableToChangeEvents);
        }
    }

    // 处理变更通知事件的虚方法
    protected virtual void ProcessChangeNotificationEvent(WindowMessageEventArgs e)
    # 如果当前运行状态为 false，直接返回
    {
        if (!Running) { return; }
        # 如果事件参数为空，抛出异常
        if (e == null) { throw new ArgumentNullException("e"); }

        # 创建一个包含通知信息的锁对象
        var notifyLock = new ChangeNotifyLock(e.Message);
        # 根据变更类型创建相应的事件参数对象
        ShellObjectNotificationEventArgs args = notifyLock.ChangeType switch
        {
            ShellObjectChangeTypes.DirectoryRename or ShellObjectChangeTypes.ItemRename => new ShellObjectRenamedEventArgs(notifyLock),
            ShellObjectChangeTypes.SystemImageUpdate => new SystemImageUpdatedEventArgs(notifyLock),
            _ => new ShellObjectChangedEventArgs(notifyLock),
        };
        # 调用事件管理器，传递当前对象、变更类型和事件参数
        _manager.Invoke(this, notifyLock.ChangeType, args);
    }

    # 事件：处理所有事件
    public event EventHandler<ShellObjectNotificationEventArgs> AllEvents
    {
        add
        {
            # 确保当前状态允许添加事件处理器
            ThrowIfRunning();
            # 向事件管理器注册处理所有事件的处理器
            _manager.Register(ShellObjectChangeTypes.AllEventsMask, value);
        }
        remove
        {
            # 确保当前状态允许移除事件处理器
            ThrowIfRunning();
            # 从事件管理器注销处理所有事件的处理器
            _manager.Unregister(ShellObjectChangeTypes.AllEventsMask, value);
        }
    }

    # 事件：处理全局事件
    public event EventHandler<ShellObjectNotificationEventArgs> GlobalEvents
    {
        add
        {
            # 确保当前状态允许添加事件处理器
            ThrowIfRunning();
            # 向事件管理器注册处理全局事件的处理器
            _manager.Register(ShellObjectChangeTypes.GlobalEventsMask, value);
        }
        remove
        {
            # 确保当前状态允许移除事件处理器
            ThrowIfRunning();
            # 从事件管理器注销处理全局事件的处理器
            _manager.Unregister(ShellObjectChangeTypes.GlobalEventsMask, value);
        }
    }

    # 事件：处理磁盘事件
    public event EventHandler<ShellObjectNotificationEventArgs> DiskEvents
    {
        add
        {
            # 确保当前状态允许添加事件处理器
            ThrowIfRunning();
            # 向事件管理器注册处理磁盘事件的处理器
            _manager.Register(ShellObjectChangeTypes.DiskEventsMask, value);
        }
        remove
        {
            # 确保当前状态允许移除事件处理器
            ThrowIfRunning();
            # 从事件管理器注销处理磁盘事件的处理器
            _manager.Unregister(ShellObjectChangeTypes.DiskEventsMask, value);
        }
    }

    # 事件：处理项目重命名事件
    public event EventHandler<ShellObjectRenamedEventArgs> ItemRenamed
    {
        add
        {
            # 确保当前状态允许添加事件处理器
            ThrowIfRunning();
            # 向事件管理器注册处理项目重命名事件的处理器
            _manager.Register(ShellObjectChangeTypes.ItemRename, value);
        }
        remove
        {
            # 确保当前状态允许移除事件处理器
            ThrowIfRunning();
            # 从事件管理器注销处理项目重命名事件的处理器
            _manager.Unregister(ShellObjectChangeTypes.ItemRename, value);
        }
    }

    # 事件：处理项目创建事件
    public event EventHandler<ShellObjectChangedEventArgs> ItemCreated
    {
        add
        {
            # 确保当前状态允许添加事件处理器
            ThrowIfRunning();
            # 向事件管理器注册处理项目创建事件的处理器
            _manager.Register(ShellObjectChangeTypes.ItemCreate, value);
        }
        remove
        {
            # 确保当前状态允许移除事件处理器
            ThrowIfRunning();
            # 从事件管理器注销处理项目创建事件的处理器
            _manager.Unregister(ShellObjectChangeTypes.ItemCreate, value);
        }
    }

    # 事件：处理项目删除事件
    public event EventHandler<ShellObjectChangedEventArgs> ItemDeleted
    {
        add
        {
            # 确保当前状态允许添加事件处理器
            ThrowIfRunning();
            # 向事件管理器注册处理项目删除事件的处理器
            _manager.Register(ShellObjectChangeTypes.ItemDelete, value);
        }
        remove
        {
            # 确保当前状态允许移除事件处理器
            ThrowIfRunning();
            # 从事件管理器注销处理项目删除事件的处理器
            _manager.Unregister(ShellObjectChangeTypes.ItemDelete, value);
        }
    }

    # 事件：处理更新事件
    public event EventHandler<ShellObjectChangedEventArgs> Updated
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.Update 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.Update, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.Update 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.Update, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> DirectoryUpdated
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.DirectoryContentsUpdate 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.DirectoryContentsUpdate, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.DirectoryContentsUpdate 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.DirectoryContentsUpdate, value);
        }
    }

    public event EventHandler<ShellObjectRenamedEventArgs> DirectoryRenamed
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.DirectoryRename 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.DirectoryRename, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.DirectoryRename 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.DirectoryRename, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> DirectoryCreated
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.DirectoryCreate 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.DirectoryCreate, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.DirectoryCreate 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.DirectoryCreate, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> DirectoryDeleted
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.DirectoryDelete 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.DirectoryDelete, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.DirectoryDelete 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.DirectoryDelete, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> MediaInserted
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.MediaInsert 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.MediaInsert, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.MediaInsert 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.MediaInsert, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> MediaRemoved
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.MediaRemove 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.MediaRemove, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.MediaRemove 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.MediaRemove, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> DriveAdded
    {
        add
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注册 ShellObjectChangeTypes.DriveAdd 类型的事件处理程序
            _manager.Register(ShellObjectChangeTypes.DriveAdd, value);
        }
        remove
        {
            # 调用 ThrowIfRunning 方法检查对象是否处于运行状态
            ThrowIfRunning();
            # 注销 ShellObjectChangeTypes.DriveAdd 类型的事件处理程序
            _manager.Unregister(ShellObjectChangeTypes.DriveAdd, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> DriveRemoved
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应驱动器移除事件
            _manager.Register(ShellObjectChangeTypes.DriveRemove, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应驱动器移除事件
            _manager.Unregister(ShellObjectChangeTypes.DriveRemove, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> FolderNetworkShared
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应文件夹网络共享事件
            _manager.Register(ShellObjectChangeTypes.NetShare, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应文件夹网络共享事件
            _manager.Unregister(ShellObjectChangeTypes.NetShare, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> FolderNetworkUnshared
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应文件夹网络取消共享事件
            _manager.Register(ShellObjectChangeTypes.NetUnshare, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应文件夹网络取消共享事件
            _manager.Unregister(ShellObjectChangeTypes.NetUnshare, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> ServerDisconnected
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应服务器断开连接事件
            _manager.Register(ShellObjectChangeTypes.ServerDisconnect, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应服务器断开连接事件
            _manager.Unregister(ShellObjectChangeTypes.ServerDisconnect, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> SystemImageChanged
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应系统镜像更改事件
            _manager.Register(ShellObjectChangeTypes.SystemImageUpdate, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应系统镜像更改事件
            _manager.Unregister(ShellObjectChangeTypes.SystemImageUpdate, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> FreeSpaceChanged
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应可用空间更改事件
            _manager.Register(ShellObjectChangeTypes.FreeSpace, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应可用空间更改事件
            _manager.Unregister(ShellObjectChangeTypes.FreeSpace, value);
        }
    }

    public event EventHandler<ShellObjectChangedEventArgs> FileTypeAssociationChanged
    {
        add
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注册事件处理器以响应文件类型关联更改事件
            _manager.Register(ShellObjectChangeTypes.AssociationChange, value);
        }
        remove
        {
            # 调用方法检查是否正在运行
            ThrowIfRunning();
            # 注销事件处理器以停止响应文件类型关联更改事件
            _manager.Unregister(ShellObjectChangeTypes.AssociationChange, value);
        }
    }

    protected virtual void Dispose(bool disposing)
    {
        # 停止所有活动
        Stop();
        # 注销所有注册的事件处理器
        _manager.UnregisterAll();

        if (_listenerHandle != 0)
        {
            # 注销消息监听器
            MessageListenerFilter.Unregister(_listenerHandle, _message);
        }
    }

    public void Dispose()
    {
        # 释放资源并通知垃圾回收器不要调用终结器
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    ~ShellObjectWatcher()
    {
        # 终结器，调用 Dispose(false) 以释放非托管资源
        Dispose(false);
    }
你提供的代码似乎是不完整的。可以提供更多上下文或具体的代码片段吗？这样我能更好地为你添加注释。
```