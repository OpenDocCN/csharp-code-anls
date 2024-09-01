# `.\better-genshin-impact\Build\MicaSetup\Helper\Setup\CertificateHelper.cs`

```cs
﻿using System.Security.Cryptography.X509Certificates;

namespace MicaSetup.Helper;

public static class CertificateHelper
{
    // 安装证书到指定的证书存储中
    public static void Install(byte[] cer, string password = null!, StoreName storeName = StoreName.Root, StoreLocation storeLocation = StoreLocation.LocalMachine)
    {
        // 创建 X509Certificate2 对象，如果提供了密码，则用密码初始化证书
        using X509Certificate2 cert = password == null ? new(cer) : new(cer, password);
        // 创建 X509Store 对象，指定证书存储的名称和位置
        using X509Store store = new(storeName, storeLocation);
        // 以读写模式打开证书存储
        store.Open(OpenFlags.ReadWrite);
        // 将证书添加到证书存储中
        store.Add(cert);
        // 关闭证书存储
        store.Close();
    }

    // 从证书文件路径安装证书到指定的证书存储中
    public static void Install(string cerFilePath, string password = null!, StoreName storeName = StoreName.Root, StoreLocation storeLocation = StoreLocation.LocalMachine)
    {
        // 创建 X509Certificate2 对象，如果提供了密码，则用密码初始化证书
        using X509Certificate2 cert = password == null ? new(cerFilePath) : new(cerFilePath, password);
        // 创建 X509Store 对象，指定证书存储的名称和位置
        using X509Store store = new(storeName, storeLocation);
        // 以读写模式打开证书存储
        store.Open(OpenFlags.ReadWrite);
        // 将证书添加到证书存储中
        store.Add(cert);
        // 关闭证书存储
        store.Close();
    }

    // 从指定的证书存储中卸载证书
    public static void Uninstall(byte[] cer, string password = null!, StoreName storeName = StoreName.Root, StoreLocation storeLocation = StoreLocation.LocalMachine)
    {
        // 创建 X509Certificate2 对象，如果提供了密码，则用密码初始化证书
        using X509Certificate2 cert = password == null ? new(cer) : new(cer, password);
        // 创建 X509Store 对象，指定证书存储的名称和位置
        using X509Store store = new(storeName, storeLocation);
        // 以读写模式打开证书存储
        store.Open(OpenFlags.ReadWrite);
        // 从证书存储中移除证书
        store.Remove(cert);
        // 关闭证书存储
        store.Close();
    }

    // 从证书文件路径卸载证书
    public static void Uninstall(string cerFilePath, string password = null!, StoreName storeName = StoreName.Root, StoreLocation storeLocation = StoreLocation.LocalMachine)
    {
        // 创建 X509Certificate2 对象，如果提供了密码，则用密码初始化证书
        using X509Certificate2 cert = password == null ? new(cerFilePath) : new(cerFilePath, password);
        // 创建 X509Store 对象，指定证书存储的名称和位置
        using X509Store store = new(storeName, storeLocation);
        // 以读写模式打开证书存储
        store.Open(OpenFlags.ReadWrite);
        // 从证书存储中移除证书
        store.Remove(cert);
        // 关闭证书存储
        store.Close();
    }
}
```