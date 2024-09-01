# `.\better-genshin-impact\Build\MicaSetup\Design\Controls\Shell\ShellPageSetting.cs`

```cs
# 引入 System 命名空间，以便使用 .NET 中的基本功能
﻿using System;
# 引入 System.Collections.Generic 命名空间，以便使用泛型集合类
using System.Collections.Generic;

# 定义一个名为 MicaSetup.Design.Controls 的命名空间
namespace MicaSetup.Design.Controls;

# 定义一个公开的类 ShellPageSetting
public class ShellPageSetting
{
    # 定义一个公开的静态字典，键为字符串，值为 Type 类型，初始化为空字典
    public static Dictionary<string, Type> PageDict = new();
}
```