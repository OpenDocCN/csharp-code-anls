# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\ScriptControlViewModel.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间
﻿using BetterGenshinImpact.Core.Config;
# 引入 BetterGenshinImpact.Core.Script.Group 命名空间
using BetterGenshinImpact.Core.Script.Group;
# 引入 BetterGenshinImpact.Core.Script.Project 命名空间
using BetterGenshinImpact.Core.Script.Project;
# 引入 BetterGenshinImpact.Service.Interface 命名空间
using BetterGenshinImpact.Service.Interface;
# 引入 BetterGenshinImpact.View.Windows 命名空间
using BetterGenshinImpact.View.Windows;
# 引入 BetterGenshinImpact.View.Windows.Editable 命名空间
using BetterGenshinImpact.View.Windows.Editable;
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间
using CommunityToolkit.Mvvm.ComponentModel;
# 引入 CommunityToolkit.Mvvm.Input 命名空间
using CommunityToolkit.Mvvm.Input;
# 引入 Microsoft.Extensions.Logging 命名空间
using Microsoft.Extensions.Logging;
# 引入 System 命名空间
using System;
# 引入 System.Collections.Generic 命名空间
using System.Collections.Generic;
# 引入 System.Collections.ObjectModel 命名空间
using System.Collections.ObjectModel;
# 引入 System.Collections.Specialized 命名空间
using System.Collections.Specialized;
# 引入 System.Diagnostics 命名空间
using System.Diagnostics;
# 引入 System.Dynamic 命名空间
using System.Dynamic;
# 引入 System.IO 命名空间
using System.IO;
# 引入 System.Linq 命名空间
using System.Linq;
# 引入 System.Threading.Tasks 命名空间
using System.Threading.Tasks;
# 引入 System.Windows 命名空间
using System.Windows;
# 引入 System.Windows.Controls 命名空间
using System.Windows.Controls;
# 引入 Wpf.Ui 命名空间
using Wpf.Ui;
# 引入 Wpf.Ui.Controls 命名空间
using Wpf.Ui.Controls;
# 引入 Wpf.Ui.Violeta.Controls 命名空间
using Wpf.Ui.Violeta.Controls;

# 定义 BetterGenshinImpact.ViewModel.Pages 命名空间
namespace BetterGenshinImpact.ViewModel.Pages;

# 定义一个公开的部分类 ScriptControlViewModel，继承自 ObservableObject，实现 INavigationAware 和 IViewModel 接口
public partial class ScriptControlViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 声明一个只读字段 _snackbarService，用于提供 Snackbar 服务
    private readonly ISnackbarService _snackbarService;

    # 声明一个只读字段 _logger，用于记录日志
    private readonly ILogger<ScriptControlViewModel> _logger = App.GetLogger<ScriptControlViewModel>();

    # 声明一个只读字段 _homePageViewModel，用于操作主页视图模型
    private readonly HomePageViewModel _homePageViewModel;

    # 声明一个只读字段 _scriptService，用于提供脚本服务
    private readonly IScriptService _scriptService;

    # 公开属性，存储配置组的集合，并标记为可观察属性
    [ObservableProperty]
    private ObservableCollection<ScriptGroup> _scriptGroups = [];

    # 公开属性，存储当前选中的配置组，并标记为可观察属性
    [ObservableProperty]
    private ScriptGroup? _selectedScriptGroup;

    # 公开字段，表示脚本组的路径
    public readonly string ScriptGroupPath = Global.Absolute(@"User\ScriptGroup");

    # 实现 INavigationAware 接口的方法，导航离开时调用
    public void OnNavigatedFrom()
    {
    }

    # 实现 INavigationAware 接口的方法，导航到此页面时调用
    public void OnNavigatedTo()
    {
        # 读取脚本组
        ReadScriptGroup();
    }

    # 构造函数，初始化 ScriptControlViewModel 类的实例
    public ScriptControlViewModel(ISnackbarService snackbarService, IScriptService scriptService, HomePageViewModel homePageViewModel)
    {
        # 通过参数注入初始化 _snackbarService
        _snackbarService = snackbarService;
        # 通过参数注入初始化 _scriptService
        _scriptService = scriptService;
        # 通过参数注入初始化 _homePageViewModel
        _homePageViewModel = homePageViewModel;
        # 订阅 ScriptGroups 集合的更改事件
        ScriptGroups.CollectionChanged += ScriptGroupsCollectionChanged;
    }

    # 标记为 RelayCommand，表示这是一个命令处理方法
    [RelayCommand]
    private void OnAddScriptGroup()
    {
        # 弹出对话框，提示用户输入配置组名称
        var str = PromptDialog.Prompt("请输入配置组名称", "新增配置组");
        # 如果用户输入了名称
        if (!string.IsNullOrEmpty(str))
        {
            # 检查 ScriptGroups 集合中是否已存在相同名称的配置组
            if (ScriptGroups.Any(x => x.Name == str))
            {
                # 显示提示消息，告知用户配置组已存在
                _snackbarService.Show(
                    "配置组已存在",
                    $"配置组 {str} 已经存在，请勿重复添加",
                    ControlAppearance.Caution,
                    null,
                    TimeSpan.FromSeconds(2)
                );
            }
            else
            {
                # 向 ScriptGroups 集合中添加新的配置组
                ScriptGroups.Add(new ScriptGroup { Name = str });
            }
        }
    }

    # 标记为 RelayCommand，表示这是一个命令处理方法
    [RelayCommand]
    public void OnDeleteScriptGroup(ScriptGroup? item)
    {
        # 如果 item 为 null，直接返回，不进行任何操作
        if (item == null)
        {
            return;
        }

        try
        {
            # 从 ScriptGroups 中移除 item
            ScriptGroups.Remove(item);
            # 删除与 item 对应的 JSON 文件
            File.Delete(Path.Combine(ScriptGroupPath, $"{item.Name}.json"));
            # 显示成功删除的提示消息
            _snackbarService.Show(
                "配置组删除成功",
                $"配置组 {item.Name} 已经被删除",
                ControlAppearance.Success,
                null,
                TimeSpan.FromSeconds(2)
            );
        }
        catch (Exception e)
        {
            # 记录删除配置组过程中发生的异常
            _logger.LogDebug(e, "删除配置组配置时失败");
            # 显示删除失败的提示消息
            _snackbarService.Show(
                "删除配置组配置失败",
                $"配置组 {item.Name} 删除失败！",
                ControlAppearance.Danger,
                null,
                TimeSpan.FromSeconds(3)
            );
        }
    }

    [RelayCommand]
    private void OnAddJsScript()
    {
        # 加载所有的 JavaScript 脚本项目
        var list = LoadAllJsScriptProjects();
        # 创建一个新的 ComboBox 控件
        var combobox = new ComboBox();

        # 将每个脚本项目添加到 ComboBox 的项中
        foreach (var scriptProject in list)
        {
            combobox.Items.Add(scriptProject.FolderName + " - " + scriptProject.Manifest.Name);
        }

        # 弹出对话框提示用户选择需要添加的 JS 脚本
        var str = PromptDialog.Prompt("请选择需要添加的JS脚本", "请选择需要添加的JS脚本", combobox);
        # 如果用户选择了有效的脚本
        if (!string.IsNullOrEmpty(str))
        {
            # 从用户选择的字符串中提取文件夹名称
            var folderName = str.Split(" - ")[0];
            # 将新的脚本项目添加到选定的脚本组中
            SelectedScriptGroup?.Projects.Add(new ScriptGroupProject(new ScriptProject(folderName)));
        }
    }

    [RelayCommand]
    private void OnAddKmScript()
    {
        # 加载所有的键鼠脚本
        var list = LoadAllKmScripts();
        # 创建一个新的 ComboBox 控件
        var combobox = new ComboBox();

        # 将每个脚本文件的名称添加到 ComboBox 的项中
        foreach (var fileInfo in list)
        {
            combobox.Items.Add(fileInfo.Name);
        }

        # 弹出对话框提示用户选择需要添加的键鼠脚本
        var str = PromptDialog.Prompt("请选择需要添加的键鼠脚本", "请选择需要添加的键鼠脚本", combobox);
        # 如果用户选择了有效的脚本
        if (!string.IsNullOrEmpty(str))
        {
            # 将新的键鼠脚本项目添加到选定的脚本组中
            SelectedScriptGroup?.Projects.Add(new ScriptGroupProject(str));
        }
    }

    private List<ScriptProject> LoadAllJsScriptProjects()
    {
        # 获取全局脚本路径
        var path = Global.ScriptPath();
        # 获取路径下所有的目录，将每个目录转换为 ScriptProject 对象
        var projects = Directory.GetDirectories(path)
            .Select(x => new ScriptProject(Path.GetFileName(x)))
            .ToList();
        # 返回脚本项目列表
        return projects;
    }

    private List<FileInfo> LoadAllKmScripts()
    {
        # 获取全局键鼠脚本文件夹的绝对路径
        var folder = Global.Absolute(@"User\KeyMouseScript");
        # 获取该文件夹下所有的文件，包括子目录中的文件
        var files = Directory.GetFiles(folder, "*.*",
            SearchOption.AllDirectories);

        # 将文件路径转换为 FileInfo 对象列表并返回
        return files.Select(file => new FileInfo(file)).ToList();
    }

    [RelayCommand]
    public void OnEditScriptCommon(ScriptGroupProject? item)
    {
        # 如果 item 为 null，直接返回，不进行任何操作
        if (item == null)
        {
            return;
        }

        # 显示编辑窗口
        ShowEditWindow(item);

        # 遍历所有脚本组，并将每个脚本组写入存储
        foreach (var group in ScriptGroups)
        {
            WriteScriptGroup(group);
        }
    }

    public static void ShowEditWindow(object viewModel)
    # 创建一个消息框对象，配置其标题、内容、关闭按钮文本和所有者窗口
    {
        var uiMessageBox = new Wpf.Ui.Controls.MessageBox
        {
            Title = "修改通用设置",  # 设置消息框标题
            Content = new ScriptGroupProjectEditor { DataContext = viewModel },  # 设置消息框内容为 ScriptGroupProjectEditor 控件，数据上下文为 viewModel
            CloseButtonText = "确定",  # 设置关闭按钮文本为“确定”
            Owner = Application.Current.MainWindow,  # 设置消息框的所有者为当前应用的主窗口
        };
        # 异步显示消息框
        uiMessageBox.ShowDialogAsync();
    }

    # RelayCommand 属性表示此方法是一个命令绑定到 UI 元素
    [RelayCommand]
    public void OnEditJsScriptSettings(ScriptGroupProject? item)
    {
        # 如果传入的 item 为 null，直接返回
        if (item == null)
        {
            return;
        }
        # 如果 item 的 Project 属性为 null，建立与项目的脚本关系
        if (item.Project == null)
        {
            item.BuildScriptProjectRelation();
        }
        # 如果 item 的 Project 属性依然为 null，直接返回
        if (item.Project == null)
        {
            return;
        }

        # 如果 item 的类型为“Javascript”
        if (item.Type == "Javascript")
        {
            # 如果 item 的 JsScriptSettingsObject 为 null，初始化为一个 ExpandoObject 对象
            if (item.JsScriptSettingsObject == null)
            {
                item.JsScriptSettingsObject = new ExpandoObject();
            }
            # 创建消息框对象，配置其标题、内容、关闭按钮文本和所有者窗口
            var uiMessageBox = new Wpf.Ui.Controls.MessageBox
            {
                Title = "修改JS脚本自定义设置",  # 设置消息框标题
                Content = item.Project.LoadSettingUi(item.JsScriptSettingsObject),  # 设置消息框内容为项目的设置 UI
                CloseButtonText = "确定",  # 设置关闭按钮文本为“确定”
                Owner = Application.Current.MainWindow,  # 设置消息框的所有者为当前应用的主窗口
            };
            # 异步显示消息框
            uiMessageBox.ShowDialogAsync();

            # 遍历 ScriptGroups 中的每一个组，调用 WriteScriptGroup 方法
            foreach (var group in ScriptGroups)
            {
                WriteScriptGroup(group);
            }
        }
        else
        {
            # 弹出警告通知，表示只有 JS 脚本才有自定义配置
            Toast.Warning("只有JS脚本才有自定义配置");
        }
    }

    # RelayCommand 属性表示此方法是一个命令绑定到 UI 元素
    [RelayCommand]
    public void OnDeleteScript(ScriptGroupProject? item)
    {
        # 如果传入的 item 为 null，直接返回
        if (item == null)
        {
            return;
        }

        # 从选定的 ScriptGroup 中移除该项目
        SelectedScriptGroup?.Projects.Remove(item);
        # 显示一个 Snackbar 通知，告知脚本配置已成功移除
        _snackbarService.Show(
            "脚本配置移除成功",  # 通知标题
            $"{item.Name} 的关联配置已经移除",  # 通知内容
            ControlAppearance.Success,  # 通知样式
            null,  # 不使用操作按钮
            TimeSpan.FromSeconds(2)  # 通知显示时间
        );
    }

    # 处理 ScriptGroups 集合变化的事件
    private void ScriptGroupsCollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
    {
        # 如果有新项被添加
        if (e.NewItems != null)
        {
            foreach (ScriptGroup newItem in e.NewItems)
            {
                # 订阅新项的 Projects 集合的变化事件
                newItem.Projects.CollectionChanged += ScriptProjectsCollectionChanged;
            }
        }

        # 如果有旧项被移除
        if (e.OldItems != null)
        {
            foreach (ScriptGroup oldItem in e.OldItems)
            {
                # 取消订阅旧项的 Projects 集合的变化事件
                oldItem.Projects.CollectionChanged -= ScriptProjectsCollectionChanged;
            }
        }

        # 更新 ScriptGroups 中每个组的排序字段
        var i = 1;
        foreach (var group in ScriptGroups)
        {
            group.Index = i++;
        }

        # 保存每个 ScriptGroup 的配置
        foreach (var group in ScriptGroups)
        {
            WriteScriptGroup(group);
        }
    }

    # 处理 ScriptProjects 集合变化的事件
    private void ScriptProjectsCollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
    {
        // 补充排序字段
        // 如果 SelectedScriptGroup 中的 Projects 数量大于 0
        if (SelectedScriptGroup is { Projects.Count: > 0 })
        {
            // 初始化索引为 1
            var i = 1;
            // 遍历 SelectedScriptGroup 的 Projects 中的每一个项目
            foreach (var project in SelectedScriptGroup.Projects)
            {
                // 设置每个项目的索引，并自增索引值
                project.Index = i++;
            }
        }

        // 保存配置组配置
        // 如果 SelectedScriptGroup 不为空
        if (SelectedScriptGroup != null)
        {
            // 调用 WriteScriptGroup 方法保存 SelectedScriptGroup 的配置
            WriteScriptGroup(SelectedScriptGroup);
        }
    }

    private void WriteScriptGroup(ScriptGroup scriptGroup)
    {
        try
        {
            // 如果 ScriptGroupPath 目录不存在
            if (!Directory.Exists(ScriptGroupPath))
            {
                // 创建 ScriptGroupPath 目录
                Directory.CreateDirectory(ScriptGroupPath);
            }

            // 构建文件路径，文件名为 scriptGroup.Name.json
            var file = Path.Combine(ScriptGroupPath, $"{scriptGroup.Name}.json");
            // 将 scriptGroup 对象转换为 JSON 并写入文件
            File.WriteAllText(file, scriptGroup.ToJson());
        }
        catch (Exception e)
        {
            // 记录保存配置组时发生的异常
            _logger.LogDebug(e, "保存配置组配置时失败");
            // 显示保存失败的通知
            _snackbarService.Show(
                "保存配置组配置失败",
                $"{scriptGroup.Name} 保存失败！",
                ControlAppearance.Danger,
                null,
                TimeSpan.FromSeconds(3)
            );
        }
    }

    private void ReadScriptGroup()
    {
        try
        {
            // 如果 ScriptGroupPath 目录不存在
            if (!Directory.Exists(ScriptGroupPath))
            {
                // 创建 ScriptGroupPath 目录
                Directory.CreateDirectory(ScriptGroupPath);
            }

            // 清空 ScriptGroups 集合
            ScriptGroups.Clear();
            // 获取 ScriptGroupPath 目录下所有 JSON 文件
            var files = Directory.GetFiles(ScriptGroupPath, "*.json");
            // 初始化 ScriptGroup 对象列表
            List<ScriptGroup> groups = [];
            // 遍历每一个文件
            foreach (var file in files)
            {
                try
                {
                    // 读取文件内容
                    var json = File.ReadAllText(file);
                    // 从 JSON 字符串创建 ScriptGroup 对象
                    var group = ScriptGroup.FromJson(json);
                    // 将创建的 ScriptGroup 对象添加到列表中
                    groups.Add(group);
                }
                catch (Exception e)
                {
                    // 记录读取单个配置组时发生的异常
                    _logger.LogDebug(e, "读取单个配置组配置时失败");
                    // 显示读取单个配置组失败的通知
                    _snackbarService.Show(
                        "读取配置组配置失败",
                        "读取配置组配置失败:" + e.Message,
                        ControlAppearance.Danger,
                        null,
                        TimeSpan.FromSeconds(3)
                    );
                }
            }

            // 按 Index 排序
            groups.Sort((a, b) => a.Index.CompareTo(b.Index));
            // 将排序后的 ScriptGroup 对象添加到 ScriptGroups 中
            foreach (var group in groups)
            {
                ScriptGroups.Add(group);
            }
        }
        catch (Exception e)
        {
            // 记录读取配置组配置时发生的异常
            _logger.LogDebug(e, "读取配置组配置时失败");
            // 显示读取配置组配置失败的通知
            _snackbarService.Show(
                "读取配置组配置失败",
                "读取配置组配置失败！",
                ControlAppearance.Danger,
                null,
                TimeSpan.FromSeconds(3)
            );
        }
    }

    [RelayCommand]
    public void OnGoToScriptGroupUrl()
    {
        // 启动默认浏览器打开指定的 URL
        Process.Start(new ProcessStartInfo("https://bgi.huiyadan.com/") { UseShellExecute = true });
    }

    [RelayCommand]
    public void OnImportScriptGroup(string scriptGroupExample)
    # 创建一个新的 ScriptGroup 对象
    {
        ScriptGroup group = new();
        # 检查 scriptGroupExample 是否等于 "AutoCrystalflyExampleGroup"
        if ("AutoCrystalflyExampleGroup" == scriptGroupExample)
        {
            # 设置 group 的 Name 属性为 "晶蝶示例组"
            group.Name = "晶蝶示例组";
            # 向 group 的 Projects 列表添加一个新的 ScriptGroupProject 对象，包含一个 ScriptProject 对象
            group.Projects.Add(new ScriptGroupProject(new ScriptProject("AutoCrystalfly")));
        }

        # 检查 ScriptGroups 中是否已经存在具有相同 Name 的组
        if (ScriptGroups.Any(x => x.Name == group.Name))
        {
            # 使用 snackbarService 显示一个警告消息，提示配置组已存在
            _snackbarService.Show(
                "配置组已存在",
                $"配置组 {group.Name} 已经存在，请勿重复添加",
                ControlAppearance.Caution,
                null,
                TimeSpan.FromSeconds(2)
            );
            # 退出当前方法
            return;
        }

        # 将新创建的 group 添加到 ScriptGroups 列表中
        ScriptGroups.Add(group);
    }

    # 标记该方法为 RelayCommand，可以在 UI 中绑定到控件事件
    [RelayCommand]
    # 异步方法，执行脚本组的操作
    public async Task OnStartScriptGroupAsync()
    {
        # 检查是否选中了一个配置组
        if (SelectedScriptGroup == null)
        {
            # 使用 snackbarService 显示一个警告消息，提示用户未选择配置组
            _snackbarService.Show(
                "未选择配置组",
                "请先选择一个配置组",
                ControlAppearance.Caution,
                null,
                TimeSpan.FromSeconds(2)
            );
            # 退出当前方法
            return;
        }

        # 调用 scriptService 的 RunMulti 方法，执行所选脚本组的项目
        await _scriptService.RunMulti(SelectedScriptGroup.Projects, SelectedScriptGroup.Name);
    }
# 结束当前代码块（如果在函数、类或控制结构中，则表示代码块的结束）
}
```