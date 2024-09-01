# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Common\ShellNativeStructs.cs`

```cs
# 引入 System 命名空间，以便使用基本的系统功能
﻿using System;
# 引入 System.Diagnostics.CodeAnalysis 命名空间，以便使用 SuppressMessage 特性
using System.Diagnostics.CodeAnalysis;

# 定义 MicaSetup.Shell.Dialogs 命名空间，以便组织相关的代码
namespace MicaSetup.Shell.Dialogs;

# 使用 Flags 特性表明该枚举可以被按位组合
[Flags]
# SuppressMessage 特性用于抑制代码分析器发出的警告
[SuppressMessage("Microsoft.Design", "CA1008:")]
# 定义一个枚举类型 AccessModes，用于表示不同的访问模式
public enum AccessModes
{
    # 直接访问模式，值为 0x00000000
    Direct = 0x00000000,
    # 事务访问模式，值为 0x00010000
    Transacted = 0x00010000,
    # 简单访问模式，值为 0x08000000
    Simple = 0x08000000,
    # 只读访问模式，值为 0x00000000
    Read = 0x00000000,
    # 只写访问模式，值为 0x00000001
    Write = 0x00000001,
    # 读写访问模式，值为 0x00000002
    ReadWrite = 0x00000002,
    # 共享模式，允许所有访问，值为 0x00000040
    ShareDenyNone = 0x00000040,
    # 共享模式，禁止读取访问，值为 0x00000030
    ShareDenyRead = 0x00000030,
    # 共享模式，禁止写入访问，值为 0x00000020
    ShareDenyWrite = 0x00000020,
    # 共享模式，独占访问，值为 0x00000010
    ShareExclusive = 0x00000010,
    # 优先级访问模式，值为 0x00040000
    Priority = 0x00040000,
    # 释放时删除，值为 0x04000000
    DeleteOnRelease = 0x04000000,
    # 不使用临时文件，值为 0x00100000
    NoScratch = 0x00100000,
    # 创建模式，值为 0x00001000
    Create = 0x00001000,
    # 转换模式，值为 0x00020000
    Convert = 0x00020000,
    # 如果已存在则失败，值为 0x00000000
    FailIfThere = 0x00000000,
    # 不创建快照，值为 0x00200000
    NoSnapshot = 0x00200000,
    # 直接单写者多读者模式，值为 0x00400000
    DirectSingleWriterMultipleReader = 0x00400000,
};
```