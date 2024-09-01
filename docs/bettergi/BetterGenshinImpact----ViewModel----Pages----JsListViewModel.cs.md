# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\JsListViewModel.cs`

```cs
# 引用所需的命名空间
﻿using BetterGenshinImpact.Core.Config;  # 引用配置相关的命名空间
using BetterGenshinImpact.Core.Script.Project;  # 引用脚本项目相关的命名空间
using BetterGenshinImpact.Service.Interface;  # 引用服务接口相关的命名空间
using CommunityToolkit.Mvvm.ComponentModel;  # 引用 MVVM 组件模型的命名空间
using CommunityToolkit.Mvvm.Input;  # 引用 MVVM 输入相关的命名空间
using Microsoft.Extensions.Logging;  # 引用日志记录相关的命名空间
using System;  # 引用基础系统功能的命名空间
using System.Collections.Generic;  # 引用通用集合功能的命名空间
using System.Collections.ObjectModel;  # 引用可观察集合功能的命名空间
using System.Diagnostics;  # 引用调试功能的命名空间
using System.IO;  # 引用输入输出功能的命名空间
using System.Threading.Tasks;  # 引用异步任务功能的命名空间
using Wpf.Ui;  # 引用 WPF 用户界面功能的命名空间
using Wpf.Ui.Controls;  # 引用 WPF 用户界面控件的命名空间
using Wpf.Ui.Violeta.Controls;  # 引用 WPF Violeta 控件的命名空间

namespace BetterGenshinImpact.ViewModel.Pages;  # 定义命名空间

# 定义 JsListViewModel 类，该类实现了 ObservableObject、INavigationAware 和 IViewModel 接口
public partial class JsListViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 声明日志记录器字段
    private readonly ILogger<JsListViewModel> _logger = App.GetLogger<JsListViewModel>();  # 初始化日志记录器
    # 声明脚本路径字段
    private readonly string scriptPath = Global.Absolute("Script");  # 设置脚本的绝对路径

    # 定义一个可观察的脚本项目集合属性
    [ObservableProperty]
    private ObservableCollection<ScriptProject> _scriptItems = [];  # 初始化脚本项目集合

    # 声明服务字段
    private ISnackbarService _snackbarService;  # 声明用于显示通知的服务
    private IScriptService _scriptService;  # 声明用于脚本处理的服务

    # 构造函数，初始化服务
    public JsListViewModel(ISnackbarService snackbarService, IScriptService scriptService)
    {
        _snackbarService = snackbarService;  # 注入并设置通知服务
        _scriptService = scriptService;  # 注入并设置脚本服务
    }

    # 初始化脚本列表视图数据
    private void InitScriptListViewData()
    {
        _scriptItems.Clear();  # 清空当前脚本项目集合
        var directoryInfos = LoadScriptFolder(scriptPath);  # 加载脚本文件夹中的目录信息
        foreach (var f in directoryInfos)  # 遍历目录信息
        {
            try
            {
                _scriptItems.Add(new ScriptProject(f.Name));  # 将每个目录项添加到脚本项目集合中
            }
            catch (Exception e)
            {
                Toast.Warning($"脚本 {f.Name} 载入失败：{e.Message}");  # 捕获并显示加载失败的警告
            }
        }
    }

    # 加载脚本文件夹中的所有子目录
    private IEnumerable<DirectoryInfo> LoadScriptFolder(string folder)
    {
        if (!Directory.Exists(folder))  # 如果文件夹不存在
        {
            Directory.CreateDirectory(folder);  # 创建文件夹
        }

        var di = new DirectoryInfo(folder);  # 创建目录信息对象

        return di.GetDirectories();  # 返回文件夹中的所有子目录
    }

    # 实现导航到此视图时的操作
    public void OnNavigatedTo()
    {
        InitScriptListViewData();  # 初始化脚本列表数据
    }

    # 实现导航离开此视图时的操作
    public void OnNavigatedFrom()
    {
    }

    # 打开脚本文件夹的命令
    [RelayCommand]
    public void OnOpenScriptsFolder()
    {
        Process.Start("explorer.exe", scriptPath);  # 使用资源管理器打开脚本文件夹
    }

    # 打开特定脚本项目文件夹的命令
    [RelayCommand]
    public void OnOpenScriptProjectFolder(ScriptProject? item)
    {
        if (item == null)  # 如果脚本项目为空
        {
            return;  # 退出方法
        }
        Process.Start("explorer.exe", item.ProjectPath);  # 使用资源管理器打开脚本项目文件夹
    }

    # 启动运行特定脚本的命令
    [RelayCommand]
    public async Task OnStartRun(ScriptProject? item)
    {
        if (item == null)  # 如果脚本项目为空
        {
            return;  # 退出方法
        }
        await _scriptService.RunMulti([item.FolderName]);  # 异步运行脚本
    }
}
```