# `.\better-genshin-impact\Build\MicaSetup\Helper\SimpleDownloadHelper.cs`

```cs
﻿using System; // 引入 System 命名空间，包含基本的系统功能
using System.Net; // 引入 System.Net 命名空间，包含网络功能

namespace MicaSetup.Helper; // 定义命名空间 MicaSetup.Helper

public static class SimpleDownloadHelper // 定义一个静态类 SimpleDownloadHelper
{
    // 定义一个静态方法 DownloadFile，用于下载文件
    public static bool DownloadFile(string address, string fileName, DownloadProgressChangedEventHandler callback = null!)
    {
        try // 开始 try 块，处理可能的异常
        {
            using WebClient client = new(); // 创建一个 WebClient 实例用于下载文件
            client.DownloadProgressChanged += (sender, e) => // 注册下载进度变化的事件处理程序
            {
                Logger.Debug($"[DownloadFile] {address} saved to '{fileName}', {e.ProgressPercentage}% completed."); // 记录下载进度的调试信息
                callback?.Invoke(sender, e); // 如果提供了回调函数，则调用它
            };

            client.DownloadFile(address, fileName); // 下载指定地址的文件并保存到指定文件名
            return true; // 如果下载成功，返回 true
        }
        catch (Exception e) // 捕获异常
        {
            Logger.Error(e); // 记录错误信息
        }
        return false; // 如果发生异常或下载失败，返回 false
    }
}
```