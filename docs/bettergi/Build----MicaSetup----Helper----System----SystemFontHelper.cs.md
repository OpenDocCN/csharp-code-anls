# `.\better-genshin-impact\Build\MicaSetup\Helper\System\SystemFontHelper.cs`

```cs
# 导入必要的命名空间
﻿using MicaSetup.Natives;
using System.Drawing;
using System.Drawing.Text;

namespace MicaSetup.Helper;

# 定义一个静态类 SystemFontHelper
public static class SystemFontHelper
{
    # 定义一个公共静态方法，用于检查是否安装了特定的字体家族
    public static bool HasFontFamily(string familyName, int? language = null!)
    {
        # 如果语言参数为空，则设置默认语言为英语（美国）
        language ??= NativeMethods.MakeLangId(PrimaryLanguageID.LANG_ENGLISH, SublanguageID.SUBLANG_ENGLISH_US);

        # 创建一个 InstalledFontCollection 实例，用于获取已安装的字体
        InstalledFontCollection installedFonts = new();

        # 遍历所有已安装的字体家族
        foreach (FontFamily fontFamily in installedFonts.Families)
        {
            # 检查当前字体家族的名称是否与给定的名称匹配
            if (fontFamily.GetName(language!.Value) == familyName)
            {
                # 如果匹配，则返回 true
                return true;
            }
        }
        # 如果没有找到匹配的字体家族，则返回 false
        return false;
    }
}
```