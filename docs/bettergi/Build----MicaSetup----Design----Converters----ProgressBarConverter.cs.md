# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\ProgressBarConverter.cs`

```cs
# 引入所需的命名空间
﻿using System;
using System.Globalization;
using System.Windows;
using System.Windows.Data;

namespace MicaSetup.Design.Converters;

# 实现 IValueConverter 接口的类，用于将进度条的状态转换为不确定状态的布尔值
internal class ProgressBarIndeterminateConverter : IValueConverter
{
    # 将输入值转换为布尔值，指示进度条是否处于不确定状态
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果输入值是 double 类型且小于 0，则表示进度条处于不确定状态
        if (value is double @double && @double < 0d)
        {
            return true;
        }
        # 否则，进度条不处于不确定状态
        return false;
    }

    # 不支持将值从目标类型转换回源类型
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}

# 实现 IValueConverter 接口的类，用于将进度条的状态转换为可见性
internal class ProgressBarValueVisibilityConverter : IValueConverter
{
    # 将输入值转换为可见性，指示进度条是否应当显示
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 如果输入值是 double 类型且小于 0，则将进度条设为隐藏
        if (value is double @double && @double < 0d)
        {
            return Visibility.Collapsed;
        }
        # 否则，将进度条设为可见
        return Visibility.Visible;
    }

    # 不支持将值从目标类型转换回源类型
    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```