# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Windows\FormViewModel.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel;  // 引入 CommunityToolkit 的 MVVM 组件模型库
using CommunityToolkit.Mvvm.Input;  // 引入 CommunityToolkit 的 MVVM 输入命令库
using System.Collections.Generic;  // 引入通用集合库，用于 List<T> 类型
using System.Collections.ObjectModel;  // 引入可观察集合库，用于 ObservableCollection<T> 类型

namespace BetterGenshinImpact.ViewModel.Windows;  // 定义命名空间

// 定义一个抽象类，泛型 T 继承自 ObservableObject，用于支持数据绑定
public abstract partial class FormViewModel<T> : ObservableObject
{
    [ObservableProperty] private ObservableCollection<T> _list;  // 定义一个私有字段，用于存储类型为 T 的可观察集合，并支持属性更改通知

    // 构造函数
    protected FormViewModel()
    {
        _list = [];  // 初始化 _list 为一个空集合
    }

    // 批量添加项到集合中
    public void AddRange(List<T> itemList)
    {
        foreach (var item in itemList)  // 遍历传入的项列表
        {
            List.Add(item);  // 将每一项添加到集合中
        }
    }

    /// <summary>
    /// 默认添加至头部
    /// </summary>
    /// <param name="item"></param>
    [RelayCommand]  // 标记此方法为命令，以便在视图中绑定
    public void OnAdd(T item)
    {
        List.Insert(0, item);  // 将项添加到集合的开头
    }

    [RelayCommand]  // 标记此方法为命令，以便在视图中绑定
    public void OnRemoveAt(int index)
    {
        List.RemoveAt(index);  // 从集合中移除指定索引的项
    }

    [RelayCommand]  // 标记此方法为命令，以便在视图中绑定
    public void OnEditAt(int index)
    {
        // 修改某行数据
        // 弹出编辑窗口
        // ...

        // 保存结果
        // List[index] = ?;  // 更新集合中指定索引的数据
    }

    [RelayCommand]  // 标记此方法为命令，以便在视图中绑定
    public void OnSave()
    {
        // 保存整个改动后的结果  // 保存当前集合的所有改动
    }
}
```