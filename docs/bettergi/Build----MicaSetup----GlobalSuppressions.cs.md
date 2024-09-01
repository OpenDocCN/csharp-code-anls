# `.\better-genshin-impact\Build\MicaSetup\GlobalSuppressions.cs`

```cs
# 引入命名空间，用于抑制代码分析器的特定警告
﻿using System.Diagnostics.CodeAnalysis;

# 使用 SuppressMessage 属性来抑制 CA1031 警告，与设计相关
[assembly: SuppressMessage("Design", "CA1031:")]
# 使用 SuppressMessage 属性来抑制 CA1062 警告，与设计相关
[assembly: SuppressMessage("Design", "CA1062:")]
```