# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\Draggable\Adorners\DoubleFormatConverter.cs`

```cs
# 引入系统基础功能的命名空间
using System;
# 引入文化相关功能的命名空间
using System.Globalization;
# 引入数据转换器接口的命名空间
using System.Windows.Data;

# 定义命名空间
namespace BetterGenshinImpact.View.Controls.Adorners;

# 实现 IValueConverter 接口的类，用于将值进行格式转换
public class DoubleFormatConverter : IValueConverter
{
    # 将源值转换为目标值的实现方法
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 将输入值转换为 double 类型
        double d = (double)value;
        # 将 double 类型值四舍五入到最接近的整数并返回
        return Math.Round(d);
    }

    # 将目标值转换为源值的实现方法，这里返回默认值
    public object? ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        # 不支持将值转换回原始类型，返回默认值
        return default;
    }
}
```