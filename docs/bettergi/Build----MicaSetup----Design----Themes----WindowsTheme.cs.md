# `.\better-genshin-impact\Build\MicaSetup\Design\Themes\WindowsTheme.cs`

```cs
# 命名空间定义，组织相关的类和接口
﻿namespace MicaSetup.Design.Controls;

# 定义一个枚举类型，表示不同的 Windows 主题
public enum WindowsTheme
{
    # 表示浅色主题
    Light,
    # 表示深色主题
    Dark,
    # 表示自动选择主题
    Auto,
}

# 定义一个枚举类型，表示不同的背景效果类型
public enum BackdropType
{
    # 表示没有背景效果
    None = 1,
    # 表示 Mica 背景效果
    Mica = 2,
    # 表示 Acrylic 背景效果
    Acrylic = 3,
    # 表示 Tabbed 背景效果
    Tabbed = 4,
}
```