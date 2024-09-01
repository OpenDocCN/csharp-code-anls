# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Common\ShellCOMClasses.cs`

```cs
# 引入 System 命名空间以使用基本的 .NET 类
﻿using System;
# 引入 System.Runtime.InteropServices 命名空间以使用 COM 互操作功能
using System.Runtime.InteropServices;

# 定义命名空间 MicaSetup.Shell.Dialogs
namespace MicaSetup.Shell.Dialogs;

# 定义 INativeShellLibrary 接口，表示一个 COM 接口
# 使用 ComImport 属性表示这个接口是从 COM 导入的
# 指定接口的 GUID
# 使用 CoClass 属性指定接口关联的 COM 类
[ComImport,
Guid(ShellIIDGuid.IShellLibrary),
CoClass(typeof(ShellLibraryCoClass))]
internal interface INativeShellLibrary : IShellLibrary
{
}

# 定义 ShellLibraryCoClass 类，表示一个 COM 类
# 使用 ComImport 属性表示这个类是从 COM 导入的
# 使用 ClassInterface 属性指定不生成类接口
# 使用 TypeLibType 属性标记该类可以被创建
# 指定类的 GUID
[ComImport,
ClassInterface(ClassInterfaceType.None),
TypeLibType(TypeLibTypeFlags.FCanCreate),
Guid(ShellCLSIDGuid.ShellLibrary)]
internal class ShellLibraryCoClass
{
}
```