# `.\better-genshin-impact\BetterGenshinImpact\ViewModel\Pages\KeyMouseRecordPageViewModel.cs`

```cs
# 导入 BetterGenshinImpact 和 CommunityToolkit.Mvvm 相关命名空间
﻿using BetterGenshinImpact.Core.Config;
using BetterGenshinImpact.Core.Recorder;
using BetterGenshinImpact.Core.Script;
using BetterGenshinImpact.GameTask;
using BetterGenshinImpact.GameTask.Model.Enum;
using BetterGenshinImpact.Model;
using BetterGenshinImpact.View.Windows;
using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;
using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Diagnostics;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Wpf.Ui;
using Wpf.Ui.Controls;
using Wpf.Ui.Violeta.Controls;

namespace BetterGenshinImpact.ViewModel.Pages;

# 定义 KeyMouseRecordPageViewModel 类，继承 ObservableObject, INavigationAware 和 IViewModel 接口
public partial class KeyMouseRecordPageViewModel : ObservableObject, INavigationAware, IViewModel
{
    # 定义私有只读日志记录器
    private readonly ILogger<KeyMouseRecordPageViewModel> _logger = App.GetLogger<KeyMouseRecordPageViewModel>();
    # 定义私有只读脚本路径
    private readonly string scriptPath = Global.Absolute(@"User\KeyMouseScript");

    # 定义观察属性，存储 KeyMouseScriptItem 集合
    [ObservableProperty]
    private ObservableCollection<KeyMouseScriptItem> _scriptItems = [];

    # 定义观察属性，标记是否正在录制
    [ObservableProperty]
    private bool _isRecording = false;

    # 定义私有 Snackbar 服务
    private ISnackbarService _snackbarService;

    # 构造函数，初始化 Snackbar 服务
    public KeyMouseRecordPageViewModel(ISnackbarService snackbarService)
    {
        _snackbarService = snackbarService;
    }

    # 初始化脚本列表视图数据
    private void InitScriptListViewData()
    {
        _scriptItems.Clear();
        # 加载脚本文件，按创建时间降序排序
        var fileInfos = LoadScriptFiles(scriptPath)
            .OrderByDescending(f => f.CreationTime)
            .ToList();
        # 遍历文件信息，添加到脚本项集合
        foreach (var f in fileInfos)
        {
            _scriptItems.Add(new KeyMouseScriptItem
            {
                Name = f.Name,
                CreateTime = f.CreationTime,
                CreateTimeStr = f.CreationTime.ToString("yyyy-MM-dd HH:mm:ss")
            });
        }
    }

    # 加载指定文件夹中的脚本文件
    private List<FileInfo> LoadScriptFiles(string folder)
    {
        # 如果文件夹不存在，则创建它
        if (!Directory.Exists(folder))
        {
            Directory.CreateDirectory(folder);
        }

        # 获取文件夹中所有文件
        var files = Directory.GetFiles(folder, "*.*",
            SearchOption.AllDirectories);

        # 将文件路径转换为 FileInfo 对象并返回
        return files.Select(file => new FileInfo(file)).ToList();
    }

    # 页面导航到此视图时调用
    public void OnNavigatedTo()
    {
        InitScriptListViewData();
    }

    # 页面导航离开此视图时调用
    public void OnNavigatedFrom()
    {
    }

    # 开始录制的命令
    [RelayCommand]
    public async Task OnStartRecord()
    {
        # 检查任务上下文是否初始化
        if (!TaskContext.Instance().IsInitialized)
        {
            Toast.Warning("请先在启动页，启动截图器再使用本功能");
            return;
        }
        # 如果当前没有录制，则开始录制
        if (!IsRecording)
        {
            IsRecording = true;
            await GlobalKeyMouseRecord.Instance.StartRecord();
        }
    }

    # 停止录制的命令
    [RelayCommand]
    public void OnStopRecord()
    {
        # 如果正在录制
        if (IsRecording)
        {
            try
            {
                # 停止录制并获取宏数据
                var macro = GlobalKeyMouseRecord.Instance.StopRecord();
                # 将宏数据保存为 JSON 文件，文件名包含当前日期时间
                File.WriteAllText(Path.Combine(scriptPath, $"BetterGI_GCM_{DateTime.Now:yyyyMMddHHmmssffff}.json"), macro);
                # 刷新脚本列表视图
                InitScriptListViewData();
                # 设置录制状态为未录制
                IsRecording = false;
            }
            catch (Exception e)
            {
                # 记录调试级别的异常信息
                _logger.LogDebug(e, "停止录制时发生异常");
                # 记录警告级别的异常消息
                _logger.LogWarning(e.Message);
            }
        }
    }

    [RelayCommand]
    public async Task OnStartPlay(string name)
    {
        # 记录开始播放的信息
        _logger.LogInformation("重放开始：{Name}", name);
        try
        {
            # 异步读取指定路径的脚本内容
            var s = await File.ReadAllTextAsync(Path.Combine(scriptPath, name));

            # 创建 TaskRunner 实例并异步运行宏播放
            await new TaskRunner(DispatcherTimerOperationEnum.UseSelfCaptureImage)
                .RunAsync(async () => await KeyMouseMacroPlayer.PlayMacro(s, CancellationContext.Instance.Cts.Token));
        }
        catch (Exception e)
        {
            # 记录错误级别的异常信息
            _logger.LogError(e, "重放脚本时发生异常");
        }
        finally
        {
            # 记录播放结束的信息
            _logger.LogInformation("重放结束：{Name}", name);
        }
    }

    [RelayCommand]
    public void OnOpenScriptFolder()
    {
        # 打开脚本所在的文件夹
        Process.Start("explorer.exe", scriptPath);
    }

    [RelayCommand]
    public void OnEditScript(KeyMouseScriptItem? item)
    {
        # 如果脚本项为空则返回
        if (item == null)
        {
            return;
        }
        try
        {
            # 弹出对话框让用户输入新的名称
            var str = PromptDialog.Prompt("请输入要修改为的名称（实际就是文件名）", "修改名称");
            if (!string.IsNullOrEmpty(str))
            {
                # 检查原始文件是否存在
                var originalFilePath = Path.Combine(scriptPath, item.Name);
                if (File.Exists(originalFilePath))
                {
                    # 重命名文件
                    File.Move(originalFilePath, Path.Combine(scriptPath, str + ".json"));
                    # 显示成功消息
                    _snackbarService.Show(
                        "修改名称成功",
                        $"脚本名称 {item.Name} 修改为 {str}",
                        ControlAppearance.Success,
                        null,
                        TimeSpan.FromSeconds(2)
                    );
                }
            }
        }
        catch (Exception)
        {
            # 显示失败消息
            _snackbarService.Show(
                "修改失败",
                $"{item.Name} 修改失败",
                ControlAppearance.Danger,
                null,
                TimeSpan.FromSeconds(3)
            );
        }
        finally
        {
            # 刷新脚本列表视图
            InitScriptListViewData();
        }
    }

    [RelayCommand]
    public void OnDeleteScript(KeyMouseScriptItem? item)
    {
        # 检查项目是否为空，如果为空则直接返回
        if (item == null)
        {
            return;
        }
        try
        {
            # 删除指定路径下的文件
            File.Delete(Path.Combine(scriptPath, item.Name));
            # 显示删除成功的消息
            _snackbarService.Show(
                "删除成功",
                $"{item.Name} 已经被删除",
                ControlAppearance.Success,
                null,
                TimeSpan.FromSeconds(2)
            );
        }
        catch (Exception)
        {
            # 捕捉异常，显示删除失败的消息
            _snackbarService.Show(
                "删除失败",
                $"{item.Name} 删除失败",
                ControlAppearance.Danger,
                null,
                TimeSpan.FromSeconds(3)
            );
        }
        finally
        {
            # 无论是否发生异常，最终都会重新初始化脚本列表视图数据
            InitScriptListViewData();
        }
    }
# 这是一个闭合的右大括号，通常用于结束一个代码块（如函数、类或控制结构）
}
```