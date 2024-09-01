# `.\better-genshin-impact\Build\MicaSetup\Helper\CryptoHelper.cs`

```cs
# 引入用于文件操作的系统 IO 类库
﻿using System.IO;
# 引入用于加密操作的安全类库
using System.Security.Cryptography;
# 引入用于编码操作的文本类库
using System.Text;

# 定义 MicaSetup.Helper 命名空间
namespace MicaSetup.Helper;

# 定义一个静态的 SHA1 加密帮助类
public static class SHA1CryptoHelper
{
    # 计算字符串的 SHA1 哈希值，并返回其十六进制表示
    public static string ComputeHash(string str, Encoding? encoding = null!)
    {
        # 使用 SHA1 算法创建哈希算法实例
        using HashAlgorithm hashAlgorithm = SHA1.Create();
        # 通过哈希算法实例计算并返回字符串的哈希值的十六进制表示
        return hashAlgorithm.ToString(str, encoding);
    }

    # 计算字节数组的 SHA1 哈希值，并返回其十六进制表示
    public static string ComputeHash(byte[] buffer)
    {
        # 使用 SHA1 算法创建哈希算法实例
        using HashAlgorithm hashAlgorithm = SHA1.Create();
        # 通过哈希算法实例计算并返回字节数组的哈希值的十六进制表示
        return hashAlgorithm.ToString(buffer);
    }

    # 计算流的 SHA1 哈希值，并返回其十六进制表示
    public static string ComputeHash(Stream inputStream)
    {
        # 使用 SHA1 算法创建哈希算法实例
        using HashAlgorithm hashAlgorithm = SHA1.Create();
        # 通过哈希算法实例计算并返回流的哈希值的十六进制表示
        return hashAlgorithm.ToString(inputStream);
    }
}

# 定义一个静态的 MD5 加密帮助类
public static class MD5CryptoHelper
{
    # 计算字符串的 MD5 哈希值，并返回其十六进制表示
    public static string ComputeHash(string str, Encoding? encoding = null!)
    {
        # 使用 MD5 算法创建哈希算法实例
        using HashAlgorithm hashAlgorithm = new MD5CryptoServiceProvider();
        # 通过哈希算法实例计算并返回字符串的哈希值的十六进制表示
        return hashAlgorithm.ToString(str, encoding);
    }

    # 计算字节数组的 MD5 哈希值，并返回其十六进制表示
    public static string ComputeHash(byte[] buffer)
    {
        # 使用 MD5 算法创建哈希算法实例
        using HashAlgorithm hashAlgorithm = new MD5CryptoServiceProvider();
        # 通过哈希算法实例计算并返回字节数组的哈希值的十六进制表示
        return hashAlgorithm.ToString(buffer);
    }

    # 计算流的 MD5 哈希值，并返回其十六进制表示
    public static string ComputeHash(Stream inputStream)
    {
        # 使用 MD5 算法创建哈希算法实例
        using HashAlgorithm hashAlgorithm = new MD5CryptoServiceProvider();
        # 通过哈希算法实例计算并返回流的哈希值的十六进制表示
        return hashAlgorithm.ToString(inputStream);
    }
}

# 定义文件级静态类扩展哈希算法的功能
file static class HashAlgorithmExtension
{
    # 扩展方法：将字节数组的哈希值转换为十六进制字符串
    public static string ToString(this HashAlgorithm self, byte[] buffer)
    {
        # 计算字节数组的哈希值
        byte[] output = self.ComputeHash(buffer);
        # 将哈希值转换为十六进制字符串
        return output.ToHexString();
    }

    # 扩展方法：将流的哈希值转换为十六进制字符串
    public static string ToString(this HashAlgorithm self, Stream inputStream)
    {
        # 计算流的哈希值
        byte[] output = self.ComputeHash(inputStream);
        # 将哈希值转换为十六进制字符串
        return output.ToHexString();
    }

    # 扩展方法：将字符串的哈希值转换为十六进制字符串
    public static string ToString(this HashAlgorithm self, string str, Encoding? encoding = null!)
    {
        # 将字符串编码为字节数组，然后计算其哈希值并转换为十六进制字符串
        return self.ToString((encoding ?? Encoding.UTF8).GetBytes(str));
    }

    # 将字节数组转换为十六进制字符串
    public static string ToHexString(this byte[] self)
    {
        # 如果字节数组为 null，返回 null
        if (self == null)
        {
            return null!;
        }
        # 如果字节数组为空，返回空字符串
        if (self.Length == 0)
        {
            return string.Empty;
        }

        # 内部静态方法：将整数值转换为十六进制字符
        static char GetHexValue(int i) => i < 10 ? (char)(i + 48) : (char)(i - 10 + 65);
        # 计算需要的字符数组长度
        int length = self.Length * 2;
        # 创建一个字符数组来存储十六进制字符串
        char[] array = new char[length];

        # 将每个字节转换为两个十六进制字符
        for (int srcIndex = 0, tarIndex = 0; srcIndex < length; srcIndex += 2)
        {
            byte b = self[tarIndex++];
            array[srcIndex] = GetHexValue(b / 16);
            array[srcIndex + 1] = GetHexValue(b % 16);
        }
        # 返回转换后的十六进制字符串
        return new string(array, 0, array.Length);
    }
}
```