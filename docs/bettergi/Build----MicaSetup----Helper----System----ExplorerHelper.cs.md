# `.\better-genshin-impact\Build\MicaSetup\Helper\System\ExplorerHelper.cs`

```cs
# 引入 MicaSetup.Natives 命名空间，提供对 Windows Shell 函数的调用
﻿using MicaSetup.Natives;
# 引入系统和运行时相关的类和方法
using System;
using System.Runtime.InteropServices;

# 定义一个静态类 ExplorerHelper 用于实现 Windows Explorer 的帮助功能
namespace MicaSetup.Helper;

public static class ExplorerHelper
{
    # 定义一个公共静态方法 Refresh，用于刷新文件关联
    public static void Refresh()
    {
        # 调用 Shell32.SHChangeNotify 方法以通知文件关联发生变化并刷新
        Shell32.SHChangeNotify(SHCNE.SHCNE_ASSOCCHANGED, SHCNF.SHCNF_FLUSH, 0, 0);
    }

    # 定义一个公共静态方法 RefreshDesktop，用于刷新桌面
    public static void RefreshDesktop()
    {
        # 调用 Shell32.SHChangeNotify 方法以通知桌面更新并刷新
        Shell32.SHChangeNotify(SHCNE.SHCNE_ASSOCCHANGED, SHCNF.SHCNF_IDLIST, 0, 0);
    }

    # 定义一个公共静态方法 Refresh，允许传递路径以更新特定目录
    public static void Refresh(string path = null!)
    {
        # 如果路径为空或为 null，则调用无参数的 Refresh 方法
        if (string.IsNullOrEmpty(path))
        {
            Refresh();
            return;
        }

        # 声明一个指向内存中字符串的指针
        nint strPtr = 0;
        try
        {
            # 将路径字符串转换为全局唯一字符串并分配内存
            strPtr = Marshal.StringToHGlobalUni(path ?? Environment.GetFolderPath(Environment.SpecialFolder.DesktopDirectory));
            # 调用 Shell32.SHChangeNotify 方法以通知目录更新
            Shell32.SHChangeNotify(SHCNE.SHCNE_UPDATEDIR, SHCNF.SHCNF_PATHW, strPtr, 0);
        }
        finally
        {
            # 释放分配的内存
            if (strPtr != 0)
            {
                Marshal.FreeHGlobal(strPtr);
            }
        }
    }
}
```