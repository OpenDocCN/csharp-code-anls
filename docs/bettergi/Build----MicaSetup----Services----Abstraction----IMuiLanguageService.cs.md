# `.\better-genshin-impact\Build\MicaSetup\Services\Abstraction\IMuiLanguageService.cs`

```cs
# 引入 Windows 媒体命名空间，用于处理字体等
﻿using System.Windows.Media;

# 定义 MicaSetup.Services 命名空间
namespace MicaSetup.Services;

# 定义一个接口 IMuiLanguageService
public interface IMuiLanguageService
{
    # 声明一个方法 GetFontFamily，返回 FontFamily 对象
    public FontFamily GetFontFamily();

    # 声明一个方法 GetXamlUriString，返回 XAML URI 字符串
    public string GetXamlUriString();

    # 声明一个方法 GetLicenseUriString，返回许可证 URI 字符串
    public string GetLicenseUriString();
}
```