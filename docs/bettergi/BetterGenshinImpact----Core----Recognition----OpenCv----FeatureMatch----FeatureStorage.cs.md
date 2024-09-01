# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\FeatureMatch\FeatureStorage.cs`

```cs
# 定义名为 FeatureStorage 的类，接收一个字符串类型的参数作为名字
public class FeatureStorage(string name)
{
    # 定义一个私有只读字符串变量 rootPath，表示数据存储的根路径
    private readonly string rootPath = Global.Absolute(@"User\Map\");

    # 设置特征类型，并将其转换为字符串
    public void SetType(Feature2DType type)
    {
        TypeName = type.ToString();
    }

    # 公开属性 TypeName，默认值为 "UNKNOWN"
    public string TypeName { get; set; } = "UNKNOWN";

    # 从指定路径加载 KeyPoint 数组，如果路径存在则反序列化 KeyPoint 数组
    public KeyPoint[]? LoadKeyPointArray()
    {
        # 调用 CreateFolder 确保目录存在
        CreateFolder();
        # 构造 KeyPoint 文件的完整路径
        string kpPath = Path.Combine(rootPath, $"{name}_{TypeName}.kp");
        # 检查文件是否存在
        if (File.Exists(kpPath))
        {
            # 创建 FileStorage 对象用于读取文件
            FileStorage fs = new(kpPath, FileStorage.Modes.Read);
            # 从 FileStorage 中读取 KeyPoint 数组
            var kpArray = fs["kp"]?.ReadKeyPoints();
            # 释放 FileStorage 资源
            fs.Release();
            # 返回读取的 KeyPoint 数组
            return kpArray;
        }
        # 如果文件不存在，返回 null
        return null;
    }

    # 将 KeyPoint 数组保存到指定路径
    public void SaveKeyPointArray(KeyPoint[] kpArray)
    {
        # 调用 CreateFolder 确保目录存在
        CreateFolder();
        # 构造 KeyPoint 文件的完整路径
        string kpPath = Path.Combine(rootPath, $"{name}_{TypeName}.kp");
        # 创建 FileStorage 对象用于写入文件
        FileStorage fs = new(kpPath, FileStorage.Modes.Write);
        # 将 KeyPoint 数组写入 FileStorage
        fs.Write("kp", kpArray);
        # 释放 FileStorage 资源
        fs.Release();
    }

    # 确保 rootPath 目录存在，不存在则创建
    private void CreateFolder()
    {
        if (Directory.Exists(rootPath) == false)
        {
            Directory.CreateDirectory(rootPath);
        }
    }
}
    // 从存储中加载描述矩阵，如果未找到则返回 null
    public Mat? LoadDescMat()
    {
        // 创建文件夹（如果不存在）
        CreateFolder();
        // 获取所有符合条件的 .mat 文件
        var files = Directory.GetFiles(rootPath, $"{name}_{TypeName}.mat", SearchOption.AllDirectories);
        // 如果没有找到文件，返回 null
        if (files.Length == 0)
        {
            return null;
        }
        // 如果找到多个文件，输出调试信息
        else if (files.Length > 1)
        {
            Debug.WriteLine($"[FeatureSerializer] Found multiple files: {string.Join(", ", files)}");
        }
        // 创建文件存储对象，读取第一个文件
        FileStorage fs = new(files[0], FileStorage.Modes.Read);
        // 从文件存储中读取名为 "desc" 的矩阵
        var mat = fs["desc"]?.ReadMat();
        // 释放文件存储对象
        fs.Release();
        // 返回读取的矩阵
        return mat;
    }

    // public void SaveDescMat1(Mat descMat)
    // {
    //     // 创建文件夹（如果不存在）
    //     CreateFolder();
    //     // 删除旧文件
    //     var files = Directory.GetFiles(rootPath, $"{name}_{TypeName}_*.mat", SearchOption.AllDirectories);
    //     foreach (var file in files)
    //     {
    //         File.Delete(file);
    //     }
    //
    //     // 生成新文件路径
    //     var descPath = Path.Combine(rootPath, $"{name}_{TypeName}_{descMat.Rows}x{descMat.Cols}.mat");
    //     // 将矩阵数据转换为字节数组
    //     var bytes = new byte[descMat.Step(0) * descMat.Rows]; // matSrcRet.Total() * matSrcRet.ElemSize()
    //     Marshal.Copy(descMat.Data, bytes, 0, bytes.Length);
    //     // 将字节数组序列化并写入文件
    //     File.WriteAllBytes(descPath, ObjectUtils.Serialize(bytes));
    // }

    // 保存描述矩阵到文件
    public void SaveDescMat(Mat descMat)
    {
        // 创建文件夹（如果不存在）
        CreateFolder();
        // 删除旧文件
        var fileName = $"{name}_{TypeName}.mat";
        var files = Directory.GetFiles(rootPath, fileName, SearchOption.AllDirectories);
        foreach (var file in files)
        {
            File.Delete(file);
        }

        // 生成新文件路径
        var descPath = Path.Combine(rootPath, fileName);
        // 创建文件存储对象，准备写入
        FileStorage fs = new(descPath, FileStorage.Modes.Write);
        // 将描述矩阵写入文件存储中，键名为 "desc"
        fs.Write("desc", descMat);
        // 释放文件存储对象
        fs.Release();
    }
你给的代码似乎不完整。你能提供更多上下文或代码片段吗？这样我才能准确地为每个语句添加注释。
```