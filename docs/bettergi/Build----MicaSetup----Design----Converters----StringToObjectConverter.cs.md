# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\StringToObjectConverter.cs`

```cs
# 使用 .NET 的系统库
﻿using System;
using System.Globalization;
using System.Windows;
using System.Windows.Data;
using System.Windows.Markup;

# 定义一个命名空间
namespace MicaSetup.Design.Converters;

# 指定值转换器，输入类型为 string，输出类型为 object
[ValueConversion(typeof(string), typeof(object))]
# 设置 XAML 内容属性为 Items
[ContentProperty(nameof(Items))]
# 定义一个将 string 转换为 object 的转换器类
public class StringToObjectConverter : SingletonConverterBase<StringToObjectConverter>
{
    # 资源字典，用于存储转换的项
    public ResourceDictionary Items { get; set; } = null!;

    # 重写 Convert 方法，实现转换逻辑
    protected override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 检查输入值是否为 string 类型
        if (value is string key)
        {
            # 如果资源字典不为空且包含指定的键，则返回对应的值
            if (this.Items != null && ContainsKey(this.Items, key))
            {
                return this.Items[key];
            }
        }

        # 如果未找到匹配的键，则返回 UnsetValue
        return UnsetValue;
    }

    # 静态方法，检查资源字典中是否包含指定的键
    private static bool ContainsKey(ResourceDictionary dict, string key)
    {
        return dict.Contains(key);
    }
}
```