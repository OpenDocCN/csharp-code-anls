# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Windows\JsonMonoViewModel.cs`

```cs
# 引入必要的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Service;
using BetterGenshinImpact.View.Windows;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using System;
using System.Linq;
using System.Text.Json;
using System.Windows;
using Wpf.Ui.Violeta.Controls;

# 定义 JsonMonoViewModel 类，并继承自 ObservableObject
namespace BetterGenshinImpact.ViewModel.Windows;

public partial class JsonMonoViewModel : ObservableObject
{
    # 声明 JSON 文本的属性
    [ObservableProperty]
    private string _jsonText = string.Empty;

    # 声明 JSON 文件路径的属性
    [ObservableProperty]
    private string _jsonPath = string.Empty;

    # 构造函数，初始化 JsonMonoViewModel 实例
    public JsonMonoViewModel(string path)
    {
        try
        {
            # 设置 JSON 文件路径
            JsonPath = path;
            # 从路径读取 JSON 文本
            JsonText = Global.ReadAllTextIfExist(JsonPath)!;
        }
        catch (Exception e)
        {
            # 读取 JSON 文件失败，显示错误信息
            MessageBox.Error("读取黑白名单出错：" + e.ToString());
        }
    }

    # 定义保存命令的方法
    [RelayCommand]
    public void Save()
    {
        try
        {
            # 尝试将 JSON 文本反序列化为对象，验证 JSON 格式是否正确
            JsonSerializer.Deserialize<object>(JsonText, ConfigService.JsonOptions);
        }
        catch (Exception e)
        {
            # 反序列化失败，显示错误信息
            MessageBox.Error("保存失败：" + e.ToString());
            # 退出方法
            return;
        }

        try
        {
            # 将 JSON 文本写入文件
            Global.WriteAllText(JsonPath, JsonText);
            # 显示保存成功的通知
            Toast.Success("保存成功");
        }
        catch (Exception e)
        {
            # 写入文件失败，显示错误信息
            MessageBox.Error("保存失败：" + e.ToString());
        }
    }

    # 定义关闭命令的方法
    [RelayCommand]
    public void Close()
    {
        # 查找并关闭带有特定标签的窗口
        Application.Current.Windows
            .Cast<Window>()
        .FirstOrDefault(w => w.Tag?.Equals(nameof(JsonMonoDialog)) ?? false)
        ?.Close();
    }
}
```