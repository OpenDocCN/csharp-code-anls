# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\ShellObjectWatcher\MessageListenerFilter.cs`

```cs
# 引入系统和集合相关的命名空间
﻿using System;
using System.Collections.Generic;
using System.Linq;

# 定义命名空间
namespace MicaSetup.Shell.Dialogs;

# 定义内部静态类 MessageListenerFilter
internal static class MessageListenerFilter
{
    # 定义静态的锁对象，用于同步
    private static readonly object _registerLock = new();
    # 定义静态的列表，用于存储已注册的监听器
    private static readonly List<RegisteredListener> _packages = new();

    # 注册消息监听器的公共方法
    public static MessageListenerFilterRegistrationResult Register(Action<WindowMessageEventArgs> callback)
    {
        # 对注册操作进行锁定，确保线程安全
        lock (_registerLock)
        {
            # 初始化消息变量
            uint message = 0;
            # 查找是否已有包注册了该回调
            var package = _packages.FirstOrDefault(x => x.TryRegister(callback, out message));
            # 如果没有找到包，则创建一个新的包并注册回调
            if (package == null)
            {
                package = new RegisteredListener();
                if (!package.TryRegister(callback, out message))
                {
                    # 如果注册失败，抛出自定义异常
                    throw new ShellException(LocalizedMessages.MessageListenerFilterUnableToRegister);
                }
                # 将新包添加到包列表中
                _packages.Add(package);
            }

            # 返回包含窗口句柄和消息的注册结果
            return new MessageListenerFilterRegistrationResult(
                package.Listener.WindowHandle,
                message);
        }
    }

    # 注销消息监听器的公共方法
    public static void Unregister(nint listenerHandle, uint message)
    {
        # 对注销操作进行锁定，确保线程安全
        lock (_registerLock)
        {
            # 查找具有指定句柄的包
            var package = _packages.FirstOrDefault(x => x.Listener.WindowHandle == listenerHandle);
            # 如果包不存在或消息未被移除，抛出异常
            if (package == null || !package.Callbacks.Remove(message))
            {
                throw new ArgumentException(LocalizedMessages.MessageListenerFilterUnknownListenerHandle);
            }

            # 如果包中的回调已全部移除，释放资源并从包列表中移除
            if (package.Callbacks.Count == 0)
            {
                package.Listener.Dispose();
                _packages.Remove(package);
            }
        }
    }

    # 内部类 RegisteredListener
    private class RegisteredListener
    # 定义一个公开的字典，用于存储消息类型到处理函数的映射
    public Dictionary<uint, Action<WindowMessageEventArgs>> Callbacks { get; private set; }

    # 定义一个公开的消息监听器
    public MessageListener Listener { get; private set; }

    # 构造函数，初始化 Callbacks 字典和 Listener 对象，并订阅 MessageReceived 事件
    public RegisteredListener()
    {
        # 初始化 Callbacks 字典
        Callbacks = new Dictionary<uint, Action<WindowMessageEventArgs>>();
        # 初始化 Listener 对象
        Listener = new MessageListener();
        # 订阅 Listener 的 MessageReceived 事件
        Listener.MessageReceived += MessageReceived;
    }

    # 消息接收事件处理函数
    private void MessageReceived(object sender, WindowMessageEventArgs e)
    {
        # 尝试从字典中获取对应的回调函数，并调用它
        if (Callbacks.TryGetValue(e.Message.Msg, out var action))
        {
            action(e);
        }
    }

    # 用于记录上一个消息的标识符，初始值为基本用户消息的标识符
    private uint _lastMessage = MessageListener.BaseUserMessage;

    # 尝试注册一个回调函数，并返回一个新的消息标识符
    public bool TryRegister(Action<WindowMessageEventArgs> callback, out uint message)
    {
        # 初始化输出参数
        message = 0;
        # 如果字典中的回调函数数量少于最大允许值
        if (Callbacks.Count < ushort.MaxValue - MessageListener.BaseUserMessage)
        {
            # 从上一个消息标识符开始，寻找可用的标识符
            var i = _lastMessage + 1;
            while (i != _lastMessage)
            {
                # 如果超出最大值，重置为基本用户消息的标识符
                if (i > ushort.MaxValue) { i = MessageListener.BaseUserMessage; }

                # 如果字典中不存在该标识符，注册回调函数
                if (!Callbacks.ContainsKey(i))
                {
                    _lastMessage = message = i;
                    Callbacks.Add(i, callback);
                    return true;
                }
                i++;
            }
        }
        # 如果找不到可用的标识符，返回 false
        return false;
    }
} # 结束当前类或方法的定义

internal class MessageListenerFilterRegistrationResult # 定义一个内部类，用于表示消息监听器过滤器注册结果
{
    internal MessageListenerFilterRegistrationResult(nint handle, uint msg) # 构造函数，接受窗口句柄和消息类型作为参数
    {
        WindowHandle = handle; # 初始化窗口句柄
        Message = msg; # 初始化消息类型
    }

    public nint WindowHandle { get; private set; } # 定义公共属性 WindowHandle，用于获取窗口句柄，且只能在类内部设置
    public uint Message { get; private set; } # 定义公共属性 Message，用于获取消息类型，且只能在类内部设置
}
```