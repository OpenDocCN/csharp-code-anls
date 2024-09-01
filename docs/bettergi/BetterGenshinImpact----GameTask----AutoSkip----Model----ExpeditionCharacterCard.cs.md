# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\Model\ExpeditionCharacterCard.cs`

```cs
# 引入 OpenCvSharp 库，用于图像处理
using OpenCvSharp;
# 引入系统集合的泛型列表
using System.Collections.Generic;

# 定义一个命名空间，用于组织代码
namespace BetterGenshinImpact.GameTask.AutoSkip.Model;

# 定义一个名为 ExpeditionCharacterCard 的公共类
public class ExpeditionCharacterCard
{
    # 可空的字符串属性，用于存储角色名称
    public string? Name { get; set; }

    # 布尔值属性，表示角色是否处于闲置状态，默认为 true
    public bool Idle { get; set; } = true;

    # 可空的字符串属性，用于存储附加信息
    public string? Addition { get; set; }

    # List 类型属性，用于存储矩形区域的列表，初始化为空列表
    public List<Rect> Rects { get; set; } = [];

    # 注释掉的构造函数，允许在实例化时设置所有属性的值
    //public ExpeditionCharacterCard(string name,string addition, bool idle)
    //{
    //    Name = name;
    //    Addition = addition;
    //    Idle = idle;
    //}
}
```