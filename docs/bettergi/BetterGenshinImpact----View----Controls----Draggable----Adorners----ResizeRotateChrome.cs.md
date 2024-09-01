# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\Adorners\ResizeRotateChrome.cs`

```cs
# 引入 WPF 框架中定义的命名空间，用于窗口和控件的实现
﻿using System.Windows;
# 引入 WPF 框架中的控件类
using System.Windows.Controls;

# 定义一个命名空间，用于组织相关的代码
namespace BetterGenshinImpact.View.Controls.Adorners;

# 定义一个名为 ResizeRotateChrome 的控件类，继承自 Control 基类
public class ResizeRotateChrome : Control
{
    # 静态构造函数，用于类初始化时设置一些静态属性或状态
    static ResizeRotateChrome()
    {
        # 重写 DefaultStyleKeyProperty 的元数据，以指定控件的默认样式
        DefaultStyleKeyProperty.OverrideMetadata(typeof(ResizeRotateChrome), new FrameworkPropertyMetadata(typeof(ResizeRotateChrome)));
    }
}
```