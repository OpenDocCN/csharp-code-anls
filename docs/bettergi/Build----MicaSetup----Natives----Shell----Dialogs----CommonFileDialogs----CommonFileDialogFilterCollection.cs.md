# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogFilterCollection.cs`

```cs
# 引入集合相关的命名空间
using System.Collections.ObjectModel;

# 定义命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义 CommonFileDialogFilterCollection 类，继承自 Collection<CommonFileDialogFilter>
public class CommonFileDialogFilterCollection : Collection<CommonFileDialogFilter>
{
    # 定义内部构造函数，初始化 CommonFileDialogFilterCollection 实例
    internal CommonFileDialogFilterCollection()
    {
    }

    # 定义内部方法 GetAllFilterSpecs，返回 FilterSpec 数组
    internal FilterSpec[] GetAllFilterSpecs()
    {
        # 创建与集合大小相同的 FilterSpec 数组
        var filterSpecs = new FilterSpec[Count];

        # 遍历集合中的每个元素
        for (var i = 0; i < Count; i++)
        {
            # 将集合中的每个 CommonFileDialogFilter 对象转换为 FilterSpec 对象
            filterSpecs[i] = this[i].GetFilterSpec();
        }

        # 返回 FilterSpec 数组
        return filterSpecs;
    }
}
```