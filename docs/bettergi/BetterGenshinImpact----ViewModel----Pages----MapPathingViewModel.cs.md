# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\MapPathingViewModel.cs`

```cs
# 引入需要的命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Core.Script.Project;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Diagnostics;
using System.IO;
using Wpf.Ui.Controls;
using Wpf.Ui.Violeta.Controls;

namespace BetterGenshinImpact.ViewModel.Pages;

# 定义 MapPathingViewModel 类，继承 ObservableObject 并实现 INavigationAware 和 IViewModel 接口
public partial class MapPathingViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 定义 Logger 和 JSON 文件路径
    private readonly ILogger<JsListViewModel> _logger = App.GetLogger<JsListViewModel>();
    private readonly string jsonPath = Global.Absolute("AutoPathing");

    # 声明 ObservableCollection 类型的脚本项目列表
    [ObservableProperty]
    private ObservableCollection<ScriptProject> _scriptItems = [];

    # 初始化脚本列表视图数据的方法
    private void InitScriptListViewData()
    {
        # 清空当前的脚本项目列表
        _scriptItems.Clear();
        # 加载脚本文件夹中的目录信息
        var directoryInfos = LoadScriptFolder(jsonPath);
        # 遍历每个目录项，尝试添加到脚本项目列表中
        foreach (var f in directoryInfos)
        {
            try
            {
                # 创建新的脚本项目并添加到列表中
                _scriptItems.Add(new ScriptProject(f.Name));
            }
            catch (Exception e)
            {
                # 显示加载失败的警告信息
                Toast.Warning($"脚本 {f.Name} 载入失败：{e.Message}");
            }
        }
    }

    # 加载脚本文件夹的方法
    private IEnumerable<DirectoryInfo> LoadScriptFolder(string folder)
    {
        # 如果文件夹不存在，则创建它
        if (!Directory.Exists(folder))
        {
            Directory.CreateDirectory(folder);
        }

        # 创建 DirectoryInfo 对象，并返回文件夹中的目录项
        var di = new DirectoryInfo(folder);
        return di.GetDirectories();
    }

    # 当导航到此视图时调用的方法
    public void OnNavigatedTo()
    {
        # 初始化脚本列表视图数据
        InitScriptListViewData();
    }

    # 当导航离开此视图时调用的方法
    public void OnNavigatedFrom()
    {
    }

    # 打开脚本文件夹的命令
    [RelayCommand]
    public void OnOpenScriptsFolder()
    {
        # 如果脚本文件夹不存在，则创建它
        if (!Directory.Exists(jsonPath))
        {
            Directory.CreateDirectory(jsonPath);
        }
        # 使用系统资源管理器打开脚本文件夹
        Process.Start("explorer.exe", jsonPath);
    }

    # 打开脚本项目文件夹的命令
    [RelayCommand]
    public void OnOpenScriptProjectFolder(ScriptProject? item)
    {
        # 如果脚本项目为空，则返回
        if (item == null)
        {
            return;
        }
        # 使用系统资源管理器打开脚本项目的文件夹
        Process.Start("explorer.exe", item.ProjectPath);
    }

    # [RelayCommand]
    # public async Task OnStartRun(ScriptProject? item)
    # {
    #     # 如果脚本项目为空，则返回
    #     if (item == null)
    #     {
    #         return;
    #     }
    #     # 执行脚本服务的多项任务
    #     await _scriptService.RunMulti([item.FolderName]);
    # }
}
```