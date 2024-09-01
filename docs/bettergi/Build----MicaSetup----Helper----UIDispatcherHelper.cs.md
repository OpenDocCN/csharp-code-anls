# `.\better-genshin-impact\Build\MicaSetup\Helper\UIDispatcherHelper.cs`

```cs
# 定义一个静态类，提供与 UI 调度器相关的帮助方法
public static class UIDispatcherHelper
{
    # 获取当前应用程序的主窗口
    public static Window MainWindow => Application.Current.Dispatcher.Invoke(() => Application.Current.MainWindow);

    # 在 UI 线程上同步调用指定的回调方法，带有可变参数
    public static void Invoke(Action callback, params object[] args)
    {
        # 调用 UI 线程上的回调方法，传递参数，忽略返回值
        _ = Application.Current?.Dispatcher.Invoke(callback, args);
    }

    # 在 UI 线程上同步调用指定的回调方法，并传递主窗口对象作为参数
    public static void Invoke(Action<Window> callback)
    {
        # 调用 UI 线程上的回调方法，将主窗口对象作为参数传递
        Application.Current?.Dispatcher.Invoke(callback, MainWindow);
    }

    # 在 UI 线程上异步调用指定的回调方法，带有可变参数
    public static void BeginInvoke(Action callback, params object[] args)
    {
        # 异步调用 UI 线程上的回调方法，传递参数，忽略返回值
        _ = Application.Current?.Dispatcher.BeginInvoke(callback, args);
    }

    # 在 UI 线程上异步调用指定的回调方法，并传递主窗口对象作为参数
    public static void BeginInvoke(Action<Window> callback)
    {
        # 异步调用 UI 线程上的回调方法，将主窗口对象作为参数传递
        Application.Current?.Dispatcher.BeginInvoke(callback, MainWindow);
    }
}
```