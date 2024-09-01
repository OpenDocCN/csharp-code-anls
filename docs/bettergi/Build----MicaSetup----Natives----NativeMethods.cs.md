# `.\better-genshin-impact\Build\MicaSetup\Natives\NativeMethods.cs`

```cs
# 定义一个包含本地方法的静态类
public static class NativeMethods
{
    # 隐藏所有窗口按钮的方法
    public static void HideAllWindowButton(nint hwnd)
    {
        # 设置窗口的样式标志，移除系统菜单按钮
        _ = User32.SetWindowLong(hwnd, (int)WindowLongFlags.GWL_STYLE, User32.GetWindowLong(hwnd, (int)WindowLongFlags.GWL_STYLE) & ~(int)WindowStyles.WS_SYSMENU);
    }

    # 设置窗口属性的方法
    public static int SetWindowAttribute(nint hwnd, DWMWINDOWATTRIBUTE attribute, int parameter)
    {
        # 调用 DWM API 设置窗口属性，并返回结果
        return DwmApi.DwmSetWindowAttribute(hwnd, attribute, ref parameter, Marshal.SizeOf<int>());
    }

    # 获取字符串资源的方法
    public static string GetStringResource(string resourceId)
    {
        # 字符串资源分割后的部分
        string[] parts;
        # 资源库名称
        string library;
        # 资源索引
        int index;

        # 如果资源 ID 为空，返回空字符串
        if (string.IsNullOrEmpty(resourceId)) { return string.Empty; }

        # 替换资源 ID 中的 "shell32,dll" 为 "shell32.dll"
        resourceId = resourceId.Replace("shell32,dll", "shell32.dll");
        # 将资源 ID 按逗号分割成部分
        parts = resourceId.Split(new char[] { ',' });

        # 获取资源库名称
        library = parts[0];
        # 去掉资源库名称中的 '@'
        library = library.Replace(@"@", string.Empty);
        # 扩展环境变量
        library = Environment.ExpandEnvironmentVariables(library);
        # 加载资源库并获取其句柄
        var handle = Kernel32.LoadLibrary(library);

        # 去掉资源索引中的 "-"
        parts[1] = parts[1].Replace("-", string.Empty);
        # 解析资源索引为整数
        index = int.Parse(parts[1], CultureInfo.InvariantCulture);

        # 创建用于接收字符串值的 StringBuilder 对象
        var stringValue = new StringBuilder(255);
        # 加载指定的字符串资源
        var retval = User32.LoadString(handle, index, stringValue, 255);

        # 返回加载的字符串，如果未成功加载，则返回 null
        return retval != 0 ? stringValue.ToString() : null!;
    }

    # 创建语言 ID 的方法
    public static ushort MakeLangId(PrimaryLanguageID primaryLangId, SublanguageID subLangId)
    {
        # 根据主语言 ID 和子语言 ID 计算语言 ID
        return (ushort)(((ushort)subLangId << 10) | (ushort)primaryLangId);
    }
}
```