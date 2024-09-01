# `.\better-genshin-impact\Build\MicaSetup\Design\Converters\Abstraction\ConverterBase.cs`

```cs
# 使用系统和数据绑定相关的库定义一个抽象的转换器基类
public abstract class ConverterBase : DependencyObject, IValueConverter
{
    # 定义一个属性，指定优选的文化设置，默认为默认的优选文化
    public PreferredCulture PreferredCulture { get; set; } = ValueConvertersConfig.DefaultPreferredCulture;

    # 抽象方法，用于将源值转换为目标类型的值，必须在子类中实现
    protected abstract object Convert(object value, Type targetType, object parameter, CultureInfo culture);

    # 虚方法，用于将目标值转换回源类型的值，默认抛出不支持的异常
    protected virtual object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotSupportedException($"Converter '{this.GetType().Name}' does not support backward conversion.");
    }

    # 实现 IValueConverter 接口的 Convert 方法，调用 Convert 方法进行转换
    object IValueConverter.Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return this.Convert(value, targetType, parameter, this.SelectCulture(() => culture));
    }

    # 实现 IValueConverter 接口的 ConvertBack 方法，调用 ConvertBack 方法进行转换
    object IValueConverter.ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return this.ConvertBack(value, targetType, parameter, this.SelectCulture(() => culture));
    }

    # 根据 PreferredCulture 属性选择合适的 CultureInfo
    private CultureInfo SelectCulture(Func<CultureInfo> converterCulture)
    {
        return PreferredCulture switch
        {
            PreferredCulture.CurrentCulture => CultureInfo.CurrentCulture,
            PreferredCulture.CurrentUICulture => CultureInfo.CurrentUICulture,
            _ => converterCulture(),
        };
    }

    # 定义一个静态只读字段，表示未设置的值
    public static readonly object UnsetValue = DependencyProperty.UnsetValue;
}
```