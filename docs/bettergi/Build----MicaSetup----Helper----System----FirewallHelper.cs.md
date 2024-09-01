# `.\better-genshin-impact\Build\MicaSetup\Helper\System\FirewallHelper.cs`

```cs
# 引入所需的命名空间
﻿using MicaSetup.Shell.NetFw;
using System;
using System.IO;
using System.Runtime.InteropServices;

namespace MicaSetup.Helper;

public static class FirewallHelper
{
    # 允许指定应用程序通过防火墙
    public static void AllowApplication(string path)
    {
        # 检查路径是否为空或 null
        if (string.IsNullOrEmpty(path))
        {
            # 如果路径为空，则抛出异常
            throw new ArgumentNullException(nameof(path));
        }
        # 检查指定路径的文件是否存在
        if (File.Exists(path) == false)
        {
            # 如果文件不存在，则抛出异常
            throw new FileNotFoundException(path);
        }

        # 获取文件名（不包括扩展名）作为规则名称
        string ruleName = Path.GetFileNameWithoutExtension(path);
        dynamic fwPolicy2 = null!;
        dynamic inboundRule = null!;

        try
        {
            # 创建防火墙策略对象
            fwPolicy2 = Activator.CreateInstance(Type.GetTypeFromProgID("HNetCfg.FwPolicy2"));

            try
            {
                # 检查是否已有相同名称的规则
                if (fwPolicy2.Rules.Item(ruleName) != null)
                {
                    # 如果规则已存在，则无需继续
                    return;
                }
            }
            catch
            {
            }

            # 创建新的防火墙规则
            inboundRule = Activator.CreateInstance(Type.GetTypeFromProgID("HNetCfg.FWRule"));
            # 启用规则
            inboundRule.Enabled = true;
            # 设置规则为允许操作
            inboundRule.Action = NET_FW_ACTION.NET_FW_ACTION_ALLOW;
            # 设置应用程序路径
            inboundRule.ApplicationName = path;
            # 设置规则名称
            inboundRule.Name = ruleName;
            # 适用于所有防火墙配置文件
            inboundRule.Profiles = (int)NET_FW_PROFILE_TYPE2.NET_FW_PROFILE2_ALL;
            # 将规则添加到防火墙策略中
            fwPolicy2.Rules.Add(inboundRule);
        }
        finally
        {
            # 释放防火墙策略对象
            if (fwPolicy2 != null)
            {
                _ = Marshal.FinalReleaseComObject(fwPolicy2);
            }
            # 释放防火墙规则对象
            if (inboundRule != null)
            {
                _ = Marshal.FinalReleaseComObject(inboundRule);
            }
        }
    }

    # 移除指定应用程序的防火墙规则
    public static void RemoveApplication(string path)
    {
        # 获取文件名（不包括扩展名）作为规则名称
        string ruleName = Path.GetFileNameWithoutExtension(path);
        dynamic fwPolicy2 = null!;
        dynamic inboundRule = null!;

        try
        {
            # 创建防火墙策略对象
            fwPolicy2 = Activator.CreateInstance(Type.GetTypeFromProgID("HNetCfg.FwPolicy2"));

            try
            {
                # 获取指定名称的规则
                inboundRule = fwPolicy2.Rules.Item(ruleName);
                if (inboundRule != null)
                {
                    # 如果规则存在，则将其移除
                    fwPolicy2.Rules.Remove(inboundRule.Name);
                }
            }
            catch
            {
            }
        }
        finally
        {
            # 释放防火墙策略对象
            if (fwPolicy2 != null)
            {
                _ = Marshal.FinalReleaseComObject(fwPolicy2);
            }
            # 释放防火墙规则对象
            if (inboundRule != null)
            {
                _ = Marshal.FinalReleaseComObject(inboundRule);
            }
        }
    }
}
```