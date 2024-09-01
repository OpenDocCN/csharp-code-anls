# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellLink.cs`

```cs
# 定义 MicaSetup.Shell.Dialogs 命名空间
﻿namespace MicaSetup.Shell.Dialogs;

# 禁用对 CS8618 警告的编译器警告（通常是与未初始化字段相关）
#pragma warning disable CS8618

# 定义一个公开类 ShellLink，继承自 ShellObject 类
public class ShellLink : ShellObject
{
    # 定义一个私有字段 _internalPath，用于存储路径信息
    private string _internalPath;

    # 定义一个内部构造函数，用于初始化 ShellLink 实例，并传入 IShellItem2 类型的 shellItem 参数
    internal ShellLink(IShellItem2 shellItem) => nativeShellItem = shellItem;

    # 定义一个虚拟属性 Path，用于获取或设置路径
    public virtual string Path
    {
        get
        {
            # 如果 _internalPath 为空且 NativeShellItem 不为空
            if (_internalPath == null && NativeShellItem != null)
            {
                # 从基类获取解析名称并赋值给 _internalPath
                _internalPath = base.ParsingName;
            }
            # 返回 _internalPath 的值（非空）
            return _internalPath!;
        }
        # 保护性设置器，用于设置 _internalPath 的值
        protected set => _internalPath = value;
    }
}
```