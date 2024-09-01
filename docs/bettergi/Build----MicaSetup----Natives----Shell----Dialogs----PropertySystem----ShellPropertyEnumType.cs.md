# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyEnumType.cs`

```cs
namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618 // 禁用警告 CS8618，可能会报告非空字段未初始化的错误

public class ShellPropertyEnumType
{
    private string displayText; // 存储显示文本
    private PropEnumType? enumType; // 存储枚举类型
    private object minValue, setValue, enumerationValue; // 存储范围的最小值、设定值和枚举值

    internal ShellPropertyEnumType(IPropertyEnumType nativePropertyEnumType) => NativePropertyEnumType = nativePropertyEnumType; // 构造函数，初始化 NativePropertyEnumType

    public string DisplayText
    {
        get
        {
            if (displayText == null) // 如果显示文本为空
            {
                NativePropertyEnumType.GetDisplayText(out displayText); // 从 NativePropertyEnumType 获取显示文本
            }
            return displayText; // 返回显示文本
        }
    }

    public PropEnumType EnumType
    {
        get
        {
            if (!enumType.HasValue) // 如果枚举类型没有值
            {
                NativePropertyEnumType.GetEnumType(out var tempEnumType); // 从 NativePropertyEnumType 获取枚举类型
                enumType = tempEnumType; // 设置枚举类型
            }
            return enumType.Value; // 返回枚举类型的值
        }
    }

    public object RangeMinValue
    {
        get
        {
            if (minValue == null) // 如果范围的最小值为空
            {
                using (var propVar = new PropVariant()) // 使用 PropVariant 实例
                {
                    NativePropertyEnumType.GetRangeMinValue(propVar); // 从 NativePropertyEnumType 获取范围的最小值
                    minValue = propVar.Value; // 设置范围的最小值
                }
            }
            return minValue; // 返回范围的最小值
        }
    }

    public object RangeSetValue
    {
        get
        {
            if (setValue == null) // 如果设定值为空
            {
                using (var propVar = new PropVariant()) // 使用 PropVariant 实例
                {
                    NativePropertyEnumType.GetRangeSetValue(propVar); // 从 NativePropertyEnumType 获取设定值
                    setValue = propVar.Value; // 设置设定值
                }
            }
            return setValue; // 返回设定值
        }
    }

    public object RangeValue
    {
        get
        {
            if (enumerationValue == null) // 如果枚举值为空
            {
                using (var propVar = new PropVariant()) // 使用 PropVariant 实例
                {
                    NativePropertyEnumType.GetValue(propVar); // 从 NativePropertyEnumType 获取枚举值
                    enumerationValue = propVar.Value; // 设置枚举值
                }
            }
            return enumerationValue; // 返回枚举值
        }
    }

    private IPropertyEnumType NativePropertyEnumType
    {
        set; // 设置 NativePropertyEnumType 属性
        get; // 获取 NativePropertyEnumType 属性
    }
}
```