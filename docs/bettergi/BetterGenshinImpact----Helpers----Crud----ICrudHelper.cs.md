# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Crud\ICrudHelper.cs`

```cs
# 引用系统集合的命名空间
﻿using System.Collections.Generic;
# 引用系统可观察集合的命名空间
using System.Collections.ObjectModel;

# 定义命名空间 BetterGenshinImpact.Helpers.Crud
namespace BetterGenshinImpact.Helpers.Crud;

# 定义泛型接口 ICrudHelper，类型参数为 T
public interface ICrudHelper<T>
{
    # 定义插入方法，接收一个实体参数，返回插入后的实体
    T Insert(T entity);

    # 定义多查询方法，返回包含 T 类型对象的可观察集合
    ObservableCollection<T> MultiQuery();

    # 定义更新方法，接收一个实体和一个条件字典，返回更新后的实体
    T Update(T entity, Dictionary<string, object> condition);

    # 定义删除方法，接收一个条件字典，返回删除是否成功的布尔值
    bool Delete(Dictionary<string, object> condition);
}
```