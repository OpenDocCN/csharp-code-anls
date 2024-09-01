# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\IconReference.cs`

```cs
﻿using System;

namespace MicaSetup.Shell.Dialogs;

public struct IconReference
{
    // 定义一个静态只读字符数组，用于分隔字符串
    private static readonly char[] commaSeparator = new char[] { ',' };
    // 模块名称字段
    private string moduleName;
    // 引用路径字段
    private string referencePath;

    // 构造函数：初始化 IconReference 对象，给定模块名称和资源 ID
    public IconReference(string moduleName, int resourceId) : this()
    {
        // 如果模块名称为空或为 null，抛出 ArgumentNullException 异常
        if (string.IsNullOrEmpty(moduleName))
        {
            throw new ArgumentNullException("moduleName");
        }

        // 设置模块名称
        this.moduleName = moduleName;
        // 设置资源 ID
        ResourceId = resourceId;
        // 格式化引用路径
        referencePath = string.Format(System.Globalization.CultureInfo.InvariantCulture,
            "{0},{1}", moduleName, resourceId);
    }

    // 构造函数：初始化 IconReference 对象，给定引用路径
    public IconReference(string refPath) : this()
    {
        // 如果引用路径为空或为 null，抛出 ArgumentNullException 异常
        if (string.IsNullOrEmpty(refPath))
        {
            throw new ArgumentNullException("refPath");
        }

        // 使用逗号分隔引用路径
        var refParams = refPath.Split(commaSeparator);

        // 如果分隔后的数组长度不为 2 或其中有空字符串，抛出 ArgumentException 异常
        if (refParams.Length != 2 || string.IsNullOrEmpty(refParams[0]) || string.IsNullOrEmpty(refParams[1]))
        {
            throw new ArgumentException(LocalizedMessages.InvalidReferencePath, "refPath");
        }

        // 设置模块名称
        moduleName = refParams[0];
        // 解析并设置资源 ID
        ResourceId = int.Parse(refParams[1], System.Globalization.CultureInfo.InvariantCulture);

        // 设置引用路径
        referencePath = refPath;
    }

    // 模块名称属性的 getter 和 setter
    public string ModuleName
    {
        get => moduleName;
        set
        {
            // 如果赋值为空或为 null，抛出 ArgumentNullException 异常
            if (string.IsNullOrEmpty(value))
            {
                throw new ArgumentNullException("value");
            }
            // 设置模块名称
            moduleName = value;
        }
    }

    // 引用路径属性的 getter 和 setter
    public string ReferencePath
    {
        get => referencePath;
        set
        {
            // 如果赋值为空或为 null，抛出 ArgumentNullException 异常
            if (string.IsNullOrEmpty(value))
            {
                throw new ArgumentNullException("value");
            }

            // 使用逗号分隔赋值的引用路径
            var refParams = value.Split(commaSeparator);

            // 如果分隔后的数组长度不为 2 或其中有空字符串，抛出 ArgumentException 异常
            if (refParams.Length != 2 || string.IsNullOrEmpty(refParams[0]) || string.IsNullOrEmpty(refParams[1]))
            {
                throw new ArgumentException(LocalizedMessages.InvalidReferencePath, "value");
            }

            // 设置模块名称
            ModuleName = refParams[0];
            // 解析并设置资源 ID
            ResourceId = int.Parse(refParams[1], System.Globalization.CultureInfo.InvariantCulture);

            // 设置引用路径
            referencePath = value;
        }
    }

    // 资源 ID 属性的 getter 和 setter
    public int ResourceId { get; set; }

    // 重载不等于运算符
    public static bool operator !=(IconReference icon1, IconReference icon2) => !(icon1 == icon2);

    // 重载等于运算符
    public static bool operator ==(IconReference icon1, IconReference icon2) => (icon1.moduleName == icon2.moduleName) &&
            (icon1.referencePath == icon2.referencePath) &&
            (icon1.ResourceId == icon2.ResourceId);

    // 重写 Equals 方法
    public override bool Equals(object obj)
    {
        // 如果对象为 null 或不是 IconReference 类型，返回 false
        if (obj == null || obj is not IconReference) { return false; }
        // 比较当前对象与传入对象是否相等
        return (this == (IconReference)obj);
    }

    // 重写 GetHashCode 方法
    {
        # 计算 moduleName 的哈希值
        var hash = moduleName.GetHashCode();
        # 将 hash 值乘以 31 并加上 referencePath 的哈希值
        hash = hash * 31 + referencePath.GetHashCode();
        # 将 hash 值乘以 31 并加上 ResourceId 的哈希值
        hash = hash * 31 + ResourceId.GetHashCode();
        # 返回最终计算得到的哈希值
        return hash;
    }
这段代码只有一个右大括号 `}`，通常它用来结束一个代码块或函数。请提供更完整的代码或上下文，以便我可以为你添加详细注释。
```