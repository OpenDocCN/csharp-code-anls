# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\Adorners\SizeChrome.cs`

```cs
# 引入 WPF 的基本类库
﻿using System.Windows;
using System.Windows.Controls;

# 定义命名空间
namespace BetterGenshinImpact.View.Controls.Adorners;

# 定义 SizeChrome 类继承自 Control
public class SizeChrome : Control
{
    # 静态构造函数，用于设置类级别的初始化
    static SizeChrome()
    {
        # 重写 DefaultStyleKeyProperty 的元数据，以指定控件的默认样式
        DefaultStyleKeyProperty.OverrideMetadata(typeof(SizeChrome), new FrameworkPropertyMetadata(typeof(SizeChrome)));
    }
}
```