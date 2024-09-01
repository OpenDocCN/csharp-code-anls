# `.\better-genshin-impact\BetterGenshinImpact\View\Converters\NotNullConverter.cs`

```cs
# 引入系统相关的命名空间
﻿using System;
# 引入用于文化信息处理的命名空间
using System.Globalization;
# 引入用于数据绑定转换的命名空间
using System.Windows.Data;

# 定义命名空间，用于组织代码
namespace BetterGenshinImpact.View.Converters
{
    # 定义一个实现了 IValueConverter 接口的类，用于进行值转换
    public class NotNullConverter : IValueConverter
    {
        # 实现 IValueConverter 接口中的 Convert 方法，用于将源数据转换为目标数据
        public object Convert(object? value, Type targetType, object? parameter, CultureInfo culture)
        {
            # 如果源数据不为空，则返回 true，否则返回 false
            return value != null;
        }

        # 实现 IValueConverter 接口中的 ConvertBack 方法，但该方法在此不被实现
        public object ConvertBack(object? value, Type targetType, object? parameter, CultureInfo culture)
        {
            # 抛出未实现异常，表示该转换方法未实现
            throw new NotImplementedException();
        }
    }
}
```