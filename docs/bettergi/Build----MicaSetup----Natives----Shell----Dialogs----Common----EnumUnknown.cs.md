# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\EnumUnknown.cs`

```cs
# 引入必要的系统库
﻿using System.Collections.Generic;
using System.Runtime.InteropServices;

namespace MicaSetup.Shell.Dialogs;

# 内部类实现 IEnumUnknown 接口
internal class EnumUnknownClass : IEnumUnknown
{
    # 存储条件对象的列表
    private readonly List<ICondition> conditionList = new List<ICondition>();
    # 当前条件的索引
    private int current = -1;

    # 构造函数，初始化条件列表
    internal EnumUnknownClass(ICondition[] conditions) => conditionList.AddRange(conditions);

    # 克隆当前 EnumUnknownClass 实例
    public HResult Clone(out IEnumUnknown result)
    {
        # 使用当前条件列表创建新实例，并将其赋值给结果参数
        result = new EnumUnknownClass(conditionList.ToArray());
        # 返回成功状态
        return HResult.Ok;
    }

    # 获取下一个条件对象
    public HResult Next(uint requestedNumber, ref nint buffer, ref uint fetchedNumber)
    {
        # 移动到下一个条件
        current++;

        # 检查是否还有更多条件
        if (current < conditionList.Count)
        {
            # 获取当前条件对象的 COM 接口指针
            buffer = Marshal.GetIUnknownForObject(conditionList[current]);
            # 设置已获取的条件数量
            fetchedNumber = 1;
            # 返回成功状态
            return HResult.Ok;
        }

        # 如果没有更多条件，则返回失败状态
        return HResult.False;
    }

    # 重置条件索引
    public HResult Reset()
    {
        # 将当前索引重置为初始值
        current = -1;
        # 返回成功状态
        return HResult.Ok;
    }

    # 跳过指定数量的条件
    public HResult Skip(uint number)
    {
        # 计算跳过后的索引
        var temp = current + (int)number;

        # 检查是否超出条件列表范围
        if (temp > (conditionList.Count - 1))
        {
            # 如果超出范围，则返回失败状态
            return HResult.False;
        }

        # 更新当前索引
        current = temp;
        # 返回成功状态
        return HResult.Ok;
    }
}
```