# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ShellObjectWatcher\ChangeNotifyEventManager.cs`

```cs
# 命名空间定义
namespace MicaSetup.Shell.Dialogs;

# 内部类定义，负责处理事件注册和通知
internal class ChangeNotifyEventManager
{
    # 静态只读字段，定义了所有可能的更改类型及其处理顺序
    private static readonly ShellObjectChangeTypes[] _changeOrder = {
        # 项目创建
        ShellObjectChangeTypes.ItemCreate,
        # 项目重命名
        ShellObjectChangeTypes.ItemRename,
        # 项目删除
        ShellObjectChangeTypes.ItemDelete,

        # 属性更改
        ShellObjectChangeTypes.AttributesChange,

        # 目录创建
        ShellObjectChangeTypes.DirectoryCreate,
        # 目录删除
        ShellObjectChangeTypes.DirectoryDelete,
        # 目录内容更新
        ShellObjectChangeTypes.DirectoryContentsUpdate,
        # 目录重命名
        ShellObjectChangeTypes.DirectoryRename,

        # 更新
        ShellObjectChangeTypes.Update,

        # 媒体插入
        ShellObjectChangeTypes.MediaInsert,
        # 媒体移除
        ShellObjectChangeTypes.MediaRemove,
        # 驱动器添加
        ShellObjectChangeTypes.DriveAdd,
        # 驱动器移除
        ShellObjectChangeTypes.DriveRemove,
        # 网络共享
        ShellObjectChangeTypes.NetShare,
        # 网络取消共享
        ShellObjectChangeTypes.NetUnshare,

        # 服务器断开连接
        ShellObjectChangeTypes.ServerDisconnect,
        # 系统镜像更新
        ShellObjectChangeTypes.SystemImageUpdate,

        # 关联更改
        ShellObjectChangeTypes.AssociationChange,
        # 空闲空间更改
        ShellObjectChangeTypes.FreeSpace,

        # 磁盘事件掩码
        ShellObjectChangeTypes.DiskEventsMask,
        # 全局事件掩码
        ShellObjectChangeTypes.GlobalEventsMask,
        # 所有事件掩码
        ShellObjectChangeTypes.AllEventsMask
    };

    # 存储注册的事件处理程序的字典
    private readonly Dictionary<ShellObjectChangeTypes, Delegate> _events = new();

    # 注册事件处理程序
    public void Register(ShellObjectChangeTypes changeType, Delegate handler)
    {
        # 尝试从字典中获取现有的委托
        if (!_events.TryGetValue(changeType, out var del))
        {
            # 如果没有，添加新的处理程序
            _events.Add(changeType, handler);
        }
        else
        {
            # 如果已有处理程序，合并新的处理程序
            del = MulticastDelegate.Combine(del, handler);
            _events[changeType] = del;
        }
    }

    # 注销事件处理程序
    public void Unregister(ShellObjectChangeTypes changeType, Delegate handler)
    {
        # 尝试从字典中获取现有的委托
        if (_events.TryGetValue(changeType, out var del))
        {
            # 从现有委托中移除指定的处理程序
            del = MulticastDelegate.Remove(del, handler);
            # 如果没有处理程序，移除字典中的条目
            if (del == null)
            {
                _events.Remove(changeType);
            }
            else
            {
                # 更新字典中的处理程序
                _events[changeType] = del;
            }
        }
    }

    # 注销所有事件处理程序
    public void UnregisterAll() => _events.Clear();

    # 触发事件处理程序
    public void Invoke(object sender, ShellObjectChangeTypes changeType, EventArgs args)
    {
        # 清除中断标志
        changeType &= ~ShellObjectChangeTypes.FromInterrupt;

        # 遍历按顺序处理所有匹配的更改类型
        foreach (var change in _changeOrder.Where(x => (x & changeType) != 0))
        {
            # 尝试获取并调用匹配更改类型的处理程序
            if (_events.TryGetValue(change, out var del))
            {
                del.DynamicInvoke(sender, args);
            }
        }
    }

    # 获取已注册的所有事件类型
    public ShellObjectChangeTypes RegisteredTypes => _events.Keys.Aggregate<ShellObjectChangeTypes, ShellObjectChangeTypes>(
                ShellObjectChangeTypes.None,
                (accumulator, changeType) => (changeType | accumulator));
}
```