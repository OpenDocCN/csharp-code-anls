# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\PropertySystem\PropertyKey.cs`

```cs
# 引入系统和互操作库
﻿using System;
using System.Runtime.InteropServices;

# 定义命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义一个结构体 PropertyKey，指定其内存布局和对齐方式
[StructLayout(LayoutKind.Sequential, Pack = 4)]
public struct PropertyKey : IEquatable<PropertyKey>
{
    # 定义 GUID 和属性 ID 字段
    private Guid formatId;
    private readonly int propertyId;

    # 获取 formatId 属性
    public Guid FormatId => formatId;

    # 获取 propertyId 属性
    public int PropertyId => propertyId;

    # 使用 GUID 和属性 ID 初始化 PropertyKey 结构体
    public PropertyKey(Guid formatId, int propertyId)
    {
        this.formatId = formatId;
        this.propertyId = propertyId;
    }

    # 使用字符串形式的 GUID 和属性 ID 初始化 PropertyKey 结构体
    public PropertyKey(string formatId, int propertyId)
    {
        this.formatId = new Guid(formatId);
        this.propertyId = propertyId;
    }

    # 实现 IEquatable 接口的 Equals 方法，用于比较两个 PropertyKey 对象是否相等
    public bool Equals(PropertyKey other) => other.Equals((object)this);

    # 重写 GetHashCode 方法，返回格式化的哈希值
    public override int GetHashCode() => formatId.GetHashCode() ^ propertyId;

    # 重写 Equals 方法，比较两个对象是否相等
    public override bool Equals(object obj)
    {
        if (obj == null)
            return false;

        if (!(obj is PropertyKey))
            return false;

        var other = (PropertyKey)obj;
        return other.formatId.Equals(formatId) && (other.propertyId == propertyId);
    }

    # 重载 == 操作符，比较两个 PropertyKey 对象是否相等
    public static bool operator ==(PropertyKey propKey1, PropertyKey propKey2) => propKey1.Equals(propKey2);

    # 重载 != 操作符，比较两个 PropertyKey 对象是否不相等
    public static bool operator !=(PropertyKey propKey1, PropertyKey propKey2) => !propKey1.Equals(propKey2);

    # 重写 ToString 方法，返回格式化的字符串表示形式
    public override string ToString() => string.Format(System.Globalization.CultureInfo.InvariantCulture,
            LocalizedMessages.PropertyKeyFormatString,
            formatId.ToString("B"), propertyId);
}
```