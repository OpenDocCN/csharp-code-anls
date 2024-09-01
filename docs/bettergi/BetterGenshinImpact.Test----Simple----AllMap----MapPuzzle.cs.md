# `.\better-genshin-impact\BetterGenshinImpact.Test\Simple\AllMap\MapPuzzle.cs`

```cs
# 引入系统诊断功能的命名空间
﻿using System.Diagnostics;
# 引入系统输入输出功能的命名空间
using System.IO;
# 引入加密功能的命名空间
using System.Security.Cryptography;
# 引入正则表达式功能的命名空间
using System.Text.RegularExpressions;
# 引入 OpenCVSharp 库的命名空间
using OpenCvSharp;

# 定义命名空间 BetterGenshinImpact.Test.Simple.AllMap
namespace BetterGenshinImpact.Test.Simple.AllMap;

# 定义 MapPuzzle 类
public class MapPuzzle
{
    # 定义静态常量 block，值为 2048
    public static readonly int block = 2048;

    # 定义静态列表 PicWhiteHashList，包含一系列的 MD5 哈希字符串
    public static List<string> PicWhiteHashList = new List<string>
    {
        "25E23B0D18C2CBEA19D28E5E399D42FA",
        "3B4847FEA7D506EAF3B75D4F4541E867",
        "CCCAF02768432A46AA0001E51DC5991B",
        "F80C609208063B289A05B8A1E0351226",
        "06767779699056515930D0597072AB7D",
        "04F757BD57FBB5FA74A6C9C0634BB122",
        "04C1EDFCD89249209D1DD885DD2F2129",
        "01B8641F5F58BBDE6E10F53D56EA7288",
        "AF56BDE27BDF534317A9FB34E08165C0",
        "05F4EAB8C6BFBADD60B8A5CCDD614F83"
    };

    # 创建一个静态 MD5 加密服务实例
    public static MD5 Md5Service = MD5.Create();

    # 定义静态方法 Put（目前为空）
    public static void Put()
    }

    # 定义 ImgInfo 类
    public class ImgInfo
    {
        # 定义 Mat 类型的属性 Img，用于存储图像
        public Mat Img { get; set; }

        # 定义 string 类型的属性 Name，用于存储图像名称
        public string Name { get; set; }

        # 定义 long 类型的属性 FileLength，用于存储文件大小
        /// <summary>
        # 文件大小
        /// </summary>
        public long FileLength { get; set; }

        # 定义 bool 类型的属性 Locked，用于标识图像是否被锁定
        public bool Locked { get; set; }

        # 定义 ImgInfo 构造函数，初始化 ImgInfo 实例
        public ImgInfo(Mat mat, string name, long fileLength, bool locked = false)
        {
            Img = mat;
            Name = name;
            FileLength = fileLength;
            Locked = locked;
        }
    }
}
```