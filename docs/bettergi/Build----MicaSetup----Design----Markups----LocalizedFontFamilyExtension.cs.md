# `.\better-genshin-impact\Build\MicaSetup\Design\Markups\LocalizedFontFamilyExtension.cs`

```cs
# 引入 MicaSetup.Services 命名空间，用于访问服务管理器
using MicaSetup.Services;
# 引入 System 命名空间，用于基本系统功能
using System;
# 引入 System.Windows.Markup 命名空间，用于 XAML 标记扩展
using System.Windows.Markup;
# 引入 System.Windows.Media 命名空间，用于字体相关功能
using System.Windows.Media;

# 定义一个命名空间 MicaSetup.Design.Markups，用于设计时的标记扩展
namespace MicaSetup.Design.Markups;

# 指定此标记扩展返回类型为 FontFamily
[MarkupExtensionReturnType(typeof(FontFamily))]
# 创建一个本地化的 FontFamily 标记扩展类
public class LocalizedFontFamilyExtension : MarkupExtension
{
    # 重写 MarkupExtension 的 ProvideValue 方法
    public override object ProvideValue(IServiceProvider serviceProvider)
    {
        # 使用服务管理器获取 IMuiLanguageService 实例，并从中获取 FontFamily
        # 如果获取服务失败或返回值为空，则返回一个新的 FontFamily 对象
        return ServiceManager.GetService<IMuiLanguageService>()?.GetFontFamily() ?? new FontFamily();
    }
}
```