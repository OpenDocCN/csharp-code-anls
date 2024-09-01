# `.\better-genshin-impact\BetterGenshinImpact\Helpers\Extensions\TaskExtension.cs`

```cs
# 定义一个命名空间为 BetterGenshinImpact.Helpers.Extensions 的类库
namespace BetterGenshinImpact.Helpers.Extensions;

# 定义一个内部静态类 TaskExtension
internal static class TaskExtension
{
    # 定义一个扩展方法 SafeForget，允许在异步方法中忽略异常
    public static async void SafeForget(this Task task)
    {
        try
        {
            # 尝试等待任务完成，不捕获任务上下文
            await task.ConfigureAwait(false);
        }
        catch (OperationCanceledException)
        {
            # 捕获并忽略操作取消异常
        }
#if DEBUG
        # 在调试模式下，捕获其他异常
        catch (Exception)
        {
            # 如果调试器附加到进程，则中断程序以便进行调试
            if (System.Diagnostics.Debugger.IsAttached)
            {
                System.Diagnostics.Debugger.Break();
            }
        }
#else
        # 在非调试模式下，捕获其他异常并忽略
        catch
        {
        }
#endif
    }
}
```