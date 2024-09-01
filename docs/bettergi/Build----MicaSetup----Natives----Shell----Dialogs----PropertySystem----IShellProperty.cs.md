# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\IShellProperty.cs`

```cs
# 定义 IShellProperty 接口，描述了一个 shell 属性的基本特征和操作
public interface IShellProperty
{
    # 获取属性的标准名称
    string CanonicalName { get; }

    # 获取属性的描述
    ShellPropertyDescription Description { get; }

    # 获取属性的图标引用
    IconReference IconReference { get; }

    # 获取属性的键
    PropertyKey PropertyKey { get; }

    # 获取属性的值，作为对象类型
    object ValueAsObject { get; }

    # 获取属性值的类型
    Type ValueType { get; }

    # 格式化属性值以便显示，根据给定的格式选项
    string FormatForDisplay(PropertyDescriptionFormatOptions format);
}
```