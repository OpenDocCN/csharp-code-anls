# `.\better-genshin-impact\Fischless.KeyboardCapture\KeyboardItem.cs`

```cs
# 定义一个命名空间 Fischless.KeyboardCapture
﻿namespace Fischless.KeyboardCapture;

# 定义一个记录结构体 KeyboardItem
public record struct KeyboardItem
{
    # 声明一个公共字段 DateTime，表示记录的时间
    public DateTime DateTime;
    # 声明一个公共字段 KeyCode，表示键盘按键的代码
    public KeyboardKeys KeyCode;
    # 声明一个公共字段 Key，表示按键的文本，可能为空
    public string? Key;

    # 定义一个构造函数，初始化 KeyboardItem 的字段
    public KeyboardItem(DateTime dateTime, KeyboardKeys keyCode, string? key = default)
    {
        # 设置 DateTime 字段的值
        DateTime = dateTime;
        # 设置 KeyCode 字段的值
        KeyCode = keyCode;
        # 设置 Key 字段的值
        Key = key;
    }
}
```