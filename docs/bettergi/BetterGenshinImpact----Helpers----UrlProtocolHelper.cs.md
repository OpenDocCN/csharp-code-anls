# `.\better-genshin-impact\BetterGenshinImpact\Helpers\UrlProtocolHelper.cs`

```cs
# 命名空间定义，用于组织代码
namespace BetterGenshinImpact.Helpers;

# 内部静态类，用于处理 URL 协议相关操作
internal static partial class UrlProtocolHelper
{
    # 定义注册表中根节点的路径
    public const string ProtocolRootKey = @"HKEY_CLASSES_ROOT\";
    # 定义当前用户注册表路径
    public const string ProtocolUserKey = @"HKEY_CURRENT_USER\" + ProtocolUserSubKey;
    # 定义用户注册表中的子键路径
    public const string ProtocolUserSubKey = @"Software\Classes\";
    # 定义协议名称
    public const string ProtocolName = "BetterGI";

    # 注册表 hive 的属性，默认为当前用户
    public static RegistryHive RegistryHive { get; set; } = RegistryHive.CurrentUser;

    # 根据注册表 hive 生成完整的协议键路径
    public static string ProtocolKeyFull => RegistryHive switch
    {
        # 如果 hive 是 ClassesRoot，则使用根键路径
        RegistryHive.ClassesRoot => ProtocolRootKey,
        # 如果 hive 是 CurrentUser 或其他，则使用用户键路径
        RegistryHive.CurrentUser or _ => ProtocolUserKey,
    } + ProtocolName;

    # 打开协议键的方法，根据不同 hive 类型选择相应的注册表路径
    private static RegistryKey? OpenSchemeKey()
    {
        # 如果 hive 是 ClassesRoot
        if (RegistryHive == RegistryHive.ClassesRoot)
        {
            # 打开 ClassesRoot 注册表根键
            using RegistryKey keyRoot = RegistryKey.OpenBaseKey(RegistryHive.ClassesRoot, RegistryView.Registry64);
            # 打开指定子键
            RegistryKey? keyScheme = keyRoot.OpenSubKey(ProtocolName);

            return keyScheme;
        }
        else
        {
            # 如果 hive 不是 ClassesRoot
            using RegistryKey keyUser = RegistryKey.OpenBaseKey(RegistryHive.CurrentUser, RegistryView.Registry64);
            # 打开用户子键
            using RegistryKey keyUserSub = keyUser.OpenSubKey(ProtocolUserSubKey)!;
            # 打开协议键
            RegistryKey? keyScheme = keyUserSub.OpenSubKey(ProtocolName);

            return keyScheme;
        }
    }

    # 创建协议键的方法，根据不同 hive 类型选择相应的注册表路径
    private static RegistryKey CreateSchemeKey()
    {
        # 如果 hive 是 ClassesRoot
        if (RegistryHive == RegistryHive.ClassesRoot)
        {
            # 打开 ClassesRoot 注册表根键
            using RegistryKey keyRoot = RegistryKey.OpenBaseKey(RegistryHive.ClassesRoot, RegistryView.Registry64);
            # 创建指定子键
            RegistryKey keyScheme = keyRoot.CreateSubKey(ProtocolName);

            return keyScheme;
        }
        else
        {
            # 如果 hive 不是 ClassesRoot
            using RegistryKey keyUser = RegistryKey.OpenBaseKey(RegistryHive.CurrentUser, RegistryView.Registry64);
            # 打开用户子键，允许写操作
            using RegistryKey keyUserSub = keyUser.OpenSubKey(ProtocolUserSubKey, true)!;
            # 创建协议键
            RegistryKey keyScheme = keyUserSub.CreateSubKey(ProtocolName);

            return keyScheme;
        }
    }

    # 根据协议名称和参数生成 URL 字符串
    public static string GetUrl(string param = null!)
    {
        return $"{ProtocolName}://{param ?? string.Empty}";
    }

    # 获取当前进程的路径并设置到输出参数
    public static bool GetPath(out string path)
    {
        if (Process.GetCurrentProcess().MainModule?.FileName is string fileName)
        {
            # 格式化路径，添加引号和参数占位符
            path = $"\"{fileName}\" \"%1\"";
            return true;
        }
        # 如果无法获取路径，设置输出参数为 null
        path = null!;
        return false;
    }

    # 判断协议是否已注册
    # 使用 OpenSchemeKey 方法打开方案键
    using RegistryKey? keyScheme = OpenSchemeKey();

    # 如果方案键不为空
    if (keyScheme != null)
    {
        # 打开 shell 子键
        using RegistryKey? keyShell = keyScheme.OpenSubKey("shell");
        # 打开 open 子键
        using RegistryKey? keyOpen = keyShell?.OpenSubKey("open");
        # 打开 command 子键
        using RegistryKey? keyCommand = keyOpen?.OpenSubKey("command");

        # 如果 command 子键的值是字符串类型
        if (keyCommand?.GetValue(string.Empty) is string pathRegistered)
        {
            # 获取路径并检查
            if (GetPath(out string path))
            {
                # 如果注册的路径与当前路径匹配
                if (pathRegistered == path)
                {
                    return true; # 返回 true 表示路径匹配
                }
            }
        }
    }
    return false; # 如果没有匹配的路径，返回 false
    # 返回方法的最终结果

    # 获取有效路径的静态方法
    public static string GetVaildPath()
    {
        # 使用 OpenSchemeKey 方法打开方案键
        using RegistryKey? keyScheme = OpenSchemeKey();

        # 如果方案键不为空
        if (keyScheme != null)
        {
            # 打开 shell 子键
            using RegistryKey? keyShell = keyScheme.OpenSubKey("shell");
            # 打开 open 子键
            using RegistryKey? keyOpen = keyShell?.OpenSubKey("open");
            # 打开 command 子键
            using RegistryKey? keyCommand = keyOpen?.OpenSubKey("command");

            # 如果 command 子键的值是字符串类型
            if (keyCommand?.GetValue(string.Empty) is string pathRegistered)
            {
                # 匹配注册的路径与可执行文件的正则表达式
                Match match = ExecutableRegex().Match(pathRegistered);

                # 如果正则表达式匹配成功且匹配组中有 exe 值
                if (match.Success && match.Groups["exe"]?.Value is string exe)
                {
                    return exe; # 返回可执行文件路径
                }
            }
        }
        return null!; # 如果没有有效路径，返回 null
    }

    # 检查协议是否有效的静态方法
    public static bool IsVaildProtocol()
    {
        # 如果获取有效路径不为 null，则协议有效
        return GetVaildPath() != null;
    }

    # 注册协议的静态方法
    public static void Register()
    {
        # 使用 CreateSchemeKey 方法创建方案键
        using RegistryKey keyScheme = CreateSchemeKey();

        # 设置方案键的值为 ProtocolName
        keyScheme.SetValue(string.Empty, ProtocolName);
        # 设置 URL Protocol 的值为空字符串
        keyScheme.SetValue("URL Protocol", string.Empty);

        # 获取路径并检查
        if (GetPath(out string path))
        {
            # 创建 shell 子键
            using RegistryKey keyShell = keyScheme.CreateSubKey("shell");
            # 创建 open 子键
            using RegistryKey keyOpen = keyShell.CreateSubKey("open");
            # 创建 command 子键
            using RegistryKey keyCommand = keyOpen.CreateSubKey("command");

            # 设置 command 子键的值为路径
            keyCommand.SetValue(string.Empty, path);
        }
    }

    # 注销协议的静态方法
    public static void Unregister()
    {
        {
            # 打开注册表根键
            using RegistryKey keyRoot = RegistryKey.OpenBaseKey(RegistryHive.ClassesRoot, RegistryView.Registry64);
            # 打开方案键
            using RegistryKey? keyScheme = keyRoot.OpenSubKey(ProtocolName, true);

            # 如果方案键不为空
            if (keyScheme != null)
            {
                keyScheme.Close(); # 关闭方案键
                keyRoot.DeleteSubKeyTree(ProtocolName); # 删除方案键及其所有子键
            }
        }
        {
            # 打开当前用户的注册表键
            using RegistryKey keyUser = RegistryKey.OpenBaseKey(RegistryHive.CurrentUser, RegistryView.Registry64);
            # 打开用户子键
            using RegistryKey keyUserSub = keyUser.OpenSubKey(ProtocolUserSubKey)!;
            # 打开方案键
            using RegistryKey? keyScheme = keyUserSub.OpenSubKey(ProtocolName, true);

            # 如果方案键不为空
            if (keyScheme != null)
            {
                keyScheme.Close(); # 关闭方案键
                keyUserSub.DeleteSubKeyTree(ProtocolName); # 删除方案键及其所有子键
            }
        }
    }

    # 注册协议的异步静态方法
    public static async Task RegisterAsync()
    # 使用异步任务运行 Register 方法
    {
        await Task.Run(() =>
        {
            // 调用 Register 方法
            Register();
        });
    }

    # 启动一个进程，打开指定的 URL
    public static void Launch(string param = null!)
    {
        // 启动一个新进程，使用系统默认的外壳执行指定的文件
        _ = Process.Start(new ProcessStartInfo()
        {
            // 使用系统外壳来执行
            UseShellExecute = true,
            // 获取要启动的文件名或 URL
            FileName = GetUrl(param),
        });
    }

    # 异步启动一个进程，打开指定的 URL
    public static async Task LaunchAsync(string param = null!)
    {
        // 在异步任务中运行 Launch 方法
        await Task.Run(() =>
        {
            // 调用 Launch 方法
            Launch(param);
        });
    }

    # 生成一个正则表达式，用于匹配特定的字符串格式
    [GeneratedRegex("\"(?<exe>[\\s\\S]*?)\" \"%1\"")]
    private static partial Regex ExecutableRegex();
这个代码片段只有一个右大括号 `}`，请提供更多代码或上下文，以便我可以更好地为你添加注释。
```