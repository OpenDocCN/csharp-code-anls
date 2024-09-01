# `.\better-genshin-impact\BetterGenshinImpact\Service\ScriptService.cs`

```cs
# 使用 BetterGenshinImpact 服务命名空间
namespace BetterGenshinImpact.Service;

# 定义 ScriptService 类，继承自 IScriptService 接口
public partial class ScriptService(HomePageViewModel homePageViewModel) : IScriptService
{
    # 声明一个用于记录日志的 ILogger 实例
    private readonly ILogger<ScriptService> _logger = App.GetLogger<ScriptService>();

    # 定义一个异步方法 RunMulti，接收文件夹名称列表和一个可选的组名称
    public async Task RunMulti(List<string> folderNameList, string? groupName = null)
    {
        # 根据文件夹名称列表创建 ScriptProject 实例，并转换为列表
        var projects = folderNameList.Select(name => new ScriptProject(name)).ToList();

        # 异步读取项目中的代码列表
        var codeList = await ReadCodeList(projects);

        # 检查代码列表中是否包含定时操作
        var hasTimer = HasTimerOperation(codeList);

        # 如果有需要，启动截图器
        await homePageViewModel.OnStartTriggerAsync();

        # 如果提供了组名称
        if (!string.IsNullOrEmpty(groupName))
        {
            # 如果包含定时操作，记录日志信息
            if (hasTimer)
            {
                _logger.LogInformation("配置组 {Name} 包含实时任务操作调用", groupName);
            }

            # 记录配置组加载完成的日志信息
            _logger.LogInformation("配置组 {Name} 加载完成，共{Cnt}个脚本，开始执行", groupName, projects.Count);
        }

        # 根据是否包含定时操作，设置定时操作类型
        var timerOperation = hasTimer ? DispatcherTimerOperationEnum.UseCacheImageWithTriggerEmpty : DispatcherTimerOperationEnum.UseSelfCaptureImage;

        # 创建 TaskRunner 实例并执行任务
        await new TaskRunner(timerOperation)
            .RunAsync(async () =>
            {
                # 遍历所有项目
                foreach (var project in projects)
                {
                    try
                    {
                        # 如果包含定时操作，清除所有触发器
                        if (hasTimer)
                        {
                            TaskTriggerDispatcher.Instance().ClearTriggers();
                        }

                        # 记录脚本执行开始的日志
                        _logger.LogInformation("------------------------------");
                        _logger.LogInformation("→ 开始执行脚本: {Name}", project.Manifest.Name);

                        # 异步执行脚本
                        await project.ExecuteAsync();

                        # 延迟 1000 毫秒
                        await Task.Delay(1000);
                    }
                    catch (Exception e)
                    {
                        # 记录调试级别的异常信息
                        _logger.LogDebug(e, "执行脚本时发生异常");

                        # 记录错误级别的异常信息
                        _logger.LogError("执行脚本时发生异常: {Msg}", e.Message);
                    }
                    finally
                    {
                        # 记录脚本执行结束的日志
                        _logger.LogInformation("→ 脚本执行结束: {Name}", project.Manifest.Name);
                        _logger.LogInformation("------------------------------");
                    }
                }
            });

        # 如果提供了组名称，记录配置组执行结束的日志
        if (!string.IsNullOrEmpty(groupName))
        {
            _logger.LogInformation("配置组 {Name} 执行结束", groupName);
        }
    }

    # 另一个异步方法 RunMulti，接收 ScriptGroupProject 列表和组名称（未实现）
    public async Task RunMulti(IEnumerable<ScriptGroupProject> projectList, string groupName)
    }
    // 定义一个异步任务方法，返回一个包含字符串的列表
    private async Task<List<string>> ReadCodeList(List<ScriptProject> list)
    {
        // 创建一个空的字符串列表，用于存储代码
        var codeList = new List<string>();
        // 遍历输入的 ScriptProject 列表
        foreach (var project in list)
        {
            // 异步加载每个项目的代码
            var code = await project.LoadCode();
            // 将加载的代码添加到 codeList 列表中
            codeList.Add(code);
        }

        // 返回包含所有代码的列表
        return codeList;
    }

    // 定义一个方法，检查代码列表中是否有定时器操作
    private bool HasTimerOperation(IEnumerable<string> codeList)
    {
        // 使用正则表达式检查代码列表中是否有符合定时器操作模式的代码
        return codeList.Any(code => DispatcherAddTimerRegex().IsMatch(code));
    }

    // 使用生成的正则表达式模式匹配不以注释开头且包含 dispatcher.addTimer 的行
    [GeneratedRegex(@"^(?!\s*\/\/)\s*dispatcher\.\s*addTimer", RegexOptions.Multiline)]
    private static partial Regex DispatcherAddTimerRegex();
这段代码只有一个右大括号 `}`，请提供更多代码以便我为其添加注释。
```