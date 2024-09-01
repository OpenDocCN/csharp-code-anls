# `.\better-genshin-impact\BetterGenshinImpact\Helpers\UIDispatcherHelper.cs`

```cs
# 定义一个静态类 UIDispatcherHelper 用于简化与 UI 线程的交互
public static class UIDispatcherHelper
{
    # 获取当前应用程序的主窗口，如果无法获取则抛出 InvalidOperationException 异常
    public static Window MainWindow => Application.Current.Dispatcher.Invoke(() => Application.Current.MainWindow) ?? throw new InvalidOperationException();

    # 在 UI 线程上同步执行给定的回调方法，允许可变参数
    public static void Invoke(Action callback, params object[] args)
    {
        _ = Application.Current.Dispatcher.Invoke(callback, args);
    }

    # 在 UI 线程上同步执行给定的回调方法，并传递 MainWindow 作为参数
    public static void Invoke(Action<Window> callback)
    {
        _ = Application.Current.Dispatcher.Invoke(callback, MainWindow);
    }

    # 在 UI 线程上同步执行给定的函数，并返回结果，要求返回类型是引用类型
    public static T Invoke<T>(Func<T> func)
        where T : class
    {
        return Application.Current.Dispatcher.Invoke(func);
    }

    # 在 UI 线程上异步执行给定的回调方法，允许可变参数
    public static void BeginInvoke(Action callback, params object[] args)
    {
        _ = Application.Current.Dispatcher.BeginInvoke(callback, args);
    }

    # 在 UI 线程上异步执行给定的回调方法，并传递 MainWindow 作为参数
    public static void BeginInvoke(Action<Window> callback)
    {
        _ = Application.Current.Dispatcher.BeginInvoke(callback, MainWindow);
    }
}
```