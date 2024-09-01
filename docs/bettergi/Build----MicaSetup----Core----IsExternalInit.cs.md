# `.\better-genshin-impact\Build\MicaSetup\Core\IsExternalInit.cs`

```cs
# 引入 System.ComponentModel 命名空间
﻿using System.ComponentModel;

# 定义一个在 System.Runtime.CompilerServices 命名空间中的类
namespace System.Runtime.CompilerServices;

/// <summary>
/// 用于记录和 "init" 关键字的解决方法类
/// </summary>
# 将此类标记为编辑器不可见，不会在设计器中显示
[EditorBrowsable(EditorBrowsableState.Never)]
# 定义一个内部类 IsExternalInit，该类无法在其他程序集访问
internal class IsExternalInit
{
}
```