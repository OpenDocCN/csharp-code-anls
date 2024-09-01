# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Crud\JsonCrudHelper.cs`

```cs
# 引入所需的命名空间
﻿using BetterGenshinImpact.Service;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.IO;
using System.Linq;
using System.Text.Json;
using System.Threading;

namespace BetterGenshinImpact.Helpers.Crud;

# 定义一个通用的 JSON CRUD 帮助类，要求 T 为类类型
public class JsonCrudHelper<T> : ICrudHelper<T> where T : class
{
    # 文件路径用于存储 JSON 数据
    private readonly string _filePath;
    # 存储对象集合的属性
    private ObservableCollection<T> Items { get; }
    # 读写锁用于线程安全
    private readonly ReaderWriterLockSlim _lock = new();

    # 构造函数，初始化文件路径并从文件加载数据
    public JsonCrudHelper(string filePath)
    {
        _filePath = filePath;
        Items = LoadFromFile();
        # 监听集合变化事件，变化时保存数据到文件
        Items.CollectionChanged += ItemsCollectionChanged;
    }

    # 当集合发生变化时，保存数据到文件
    private void ItemsCollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
    {
        _lock.EnterWriteLock();
        try
        {
            SaveToFile();
        }
        finally
        {
            _lock.ExitWriteLock();
        }
    }

    # 插入新实体到集合并保存到文件
    public T Insert(T entity)
    {
        _lock.EnterWriteLock();
        Items.Add(entity);
        SaveToFile();
        return entity;
    }

    # 读取当前所有集合项
    public ObservableCollection<T> MultiQuery()
    {
        _lock.EnterReadLock();
        try
        {
            return Items;
        }
        finally
        {
            _lock.ExitReadLock();
        }
    }

    # 更新满足条件的实体
    public T Update(T entity, Dictionary<string, object> condition)
    {
        var index = Items.ToList().FindIndex(e => MatchesCondition(e, condition));
        if (index >= 0)
        {
            Items[index] = entity;
            return entity;
        }
        throw new KeyNotFoundException("Entity not found with the given condition.");
    }

    # 删除满足条件的实体
    public bool Delete(Dictionary<string, object> condition)
    {
        var index = Items.ToList().FindIndex(e => MatchesCondition(e, condition));
        if (index >= 0)
        {
            Items.RemoveAt(index);
            return true;
        }
        return false;
    }

    # 从文件加载对象集合
    private ObservableCollection<T> LoadFromFile()
    {
        if (!File.Exists(_filePath))
        {
            return [];
        }

        var json = File.ReadAllText(_filePath);
        return JsonSerializer.Deserialize<ObservableCollection<T>>(json, ConfigService.JsonOptions) ?? [];
    }

    # 保存对象集合到文件
    private void SaveToFile()
    {
        var json = JsonSerializer.Serialize(Items, ConfigService.JsonOptions);
        File.WriteAllText(_filePath, json);
    }

    # 检查实体是否满足指定条件
    private bool MatchesCondition(T entity, Dictionary<string, object> condition)
    {
        foreach (var kvp in condition)
        {
            var property = typeof(T).GetProperty(kvp.Key);
            if (property == null || !kvp.Value.Equals(property.GetValue(entity)))
            {
                return false;
            }
        }
        return true;
    }
}
```