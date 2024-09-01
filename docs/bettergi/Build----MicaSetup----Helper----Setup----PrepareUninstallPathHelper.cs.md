# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\PrepareUninstallPathHelper.cs`

```cs
# 定义命名空间 MicaSetup.Helper
﻿namespace MicaSetup.Helper;

# 定义一个静态类 PrepareUninstallPathHelper
public static class PrepareUninstallPathHelper
{
    # 定义一个静态方法 GetPrepareUninstallPath，接收一个字符串参数 keyName，返回 UninstallDataInfo 类型
    public static UninstallDataInfo GetPrepareUninstallPath(string keyName)
    {
        try
        {
            # 调用 RegistyUninstallHelper 的 Read 方法读取与 keyName 相关的卸载信息，存储在 UninstallInfo 类型的变量 info 中
            UninstallInfo info = RegistyUninstallHelper.Read(keyName);
            # 创建一个 UninstallDataInfo 类型的新对象 uinfo，并初始化其 InstallLocation 和 UninstallData 属性
            UninstallDataInfo uinfo = new()
            {
                InstallLocation = info.InstallLocation,
                UninstallData = info.UninstallData,
            };
            # 返回初始化后的 UninstallDataInfo 对象 uinfo
            return uinfo;
        }
        catch
        {
            # 捕获所有异常，但不做任何处理
        }
        # 返回一个非空的默认值，避免方法返回空值的警告
        return null!;
    }
}
```