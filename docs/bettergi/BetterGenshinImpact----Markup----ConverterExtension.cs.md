# `.\better-genshin-impact\BetterGenshinImpact\Markup\ConverterExtension.cs`

```cs
# 使用 MarkupExtension 提供 XAML 中的自定义标记扩展
[MarkupExtensionReturnType(typeof(object))]
public class ConverterExtension : MarkupExtension
{
    # 执行转换操作所需的输入值
    public object? Value { get; set; } = null;

    # 实现转换逻辑的转换器
    public IValueConverter Converter { get; set; } = null!;

    # 传递给转换器的额外参数
    public object? Parameter { get; set; } = null;

    # 执行转换时使用的文化信息
    public CultureInfo Culture { get; set; } = null!;

    # 构造函数，初始化 ConverterExtension 类的实例
    public ConverterExtension()
    {
    }

    # 重写 ProvideValue 方法以提供 XAML 中使用的值
    public override object ProvideValue(IServiceProvider serviceProvider)
    {
        # 如果没有提供转换器，则返回原始值
        if (Converter == null)
        {
            return Value!;
        }
        else
        {
            # 默认的目标类型为 object
            Type targetType = typeof(object);

            # 尝试从服务提供者获取 IProvideValueTarget 服务
            if (serviceProvider.GetService(typeof(IProvideValueTarget)) is IProvideValueTarget provideValueTarget)
            {
                # 获取目标属性
                var targetProperty = provideValueTarget.TargetProperty;

                # 如果目标属性是 DependencyProperty，则获取其类型
                if (targetProperty is DependencyProperty dp)
                {
                    targetType = dp.PropertyType;
                }
                # 如果目标属性是 PropertyInfo，则获取其类型
                else if (targetProperty is PropertyInfo prop)
                {
                    targetType = prop.PropertyType;
                }
            }

            # 使用转换器将值转换为目标类型，并返回转换后的结果
            return Converter.Convert(Value, targetType, Parameter, Culture);
        }
    }
}
```