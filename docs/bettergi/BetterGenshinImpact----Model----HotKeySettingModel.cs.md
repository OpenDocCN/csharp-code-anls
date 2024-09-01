# `.\better-genshin-impact\BetterGenshinImpact\Model\HotKeySettingModel.cs`

```cs
﻿using CommunityToolkit.Mvvm.ComponentModel;  // 引入 MVVM 组件模型库，用于简化数据绑定和属性通知
using CommunityToolkit.Mvvm.Input;  // 引入 MVVM 输入库，提供命令功能
using Fischless.HotkeyCapture;  // 引入热键捕捉库，支持热键监听
using System;  // 引入系统基本功能
using System.Diagnostics;  // 引入调试功能
using System.Windows.Forms;  // 引入 Windows 表单功能
using System.Windows.Input;  // 引入 WPF 输入功能

namespace BetterGenshinImpact.Model;  // 定义命名空间

/// <summary>
/// 在页面展示快捷键配置的对象
/// </summary>
public partial class HotKeySettingModel : ObservableObject  // 定义热键设置模型类，继承自可观察对象
{
    [ObservableProperty] private HotKey _hotKey;  // 定义快捷键属性，并使其可观察

    /// <summary>
    /// 键鼠监听、全局热键
    /// </summary>
    [ObservableProperty] private HotKeyTypeEnum _hotKeyType;  // 定义热键类型属性，并使其可观察

    [ObservableProperty] private string _hotKeyTypeName;  // 定义热键类型名称属性，并使其可观察

    public string FunctionName { get; set; }  // 定义功能名称属性

    public string ConfigPropertyName { get; set; }  // 定义配置属性名称

    public Action<object?, KeyPressedEventArgs>? OnKeyPressAction { get; set; }  // 定义按键按下时的回调动作

    public Action<object?, KeyPressedEventArgs>? OnKeyDownAction { get; set; }  // 定义按键按下时的回调动作

    public Action<object?, KeyPressedEventArgs>? OnKeyUpAction { get; set; }  // 定义按键抬起时的回调动作

    public bool IsHold { get; set; }  // 定义是否为长按状态

    [ObservableProperty] private bool _switchHotkeyTypeEnabled;  // 定义切换热键类型是否启用的属性，并使其可观察

    /// <summary>
    /// 全局热键配置
    /// </summary>
    public HotkeyHook? GlobalRegisterHook { get; set; }  // 定义全局热键钩子配置

    /// <summary>
    /// 键盘监听配置
    /// </summary>
    public KeyboardHook? KeyboardMonitorHook { get; set; }  // 定义键盘监听钩子配置

    /// <summary>
    /// 鼠标监听配置
    /// </summary>
    public MouseHook? MouseMonitorHook { get; set; }  // 定义鼠标监听钩子配置

    public HotKeySettingModel(string functionName, string configPropertyName, string hotkey, string hotKeyTypeCode, Action<object?, KeyPressedEventArgs>? onKeyPressAction, bool isHold = false)  // 定义构造函数
    {
        FunctionName = functionName;  // 初始化功能名称
        ConfigPropertyName = configPropertyName;  // 初始化配置属性名称
        HotKey = HotKey.FromString(hotkey);  // 从字符串初始化快捷键
        HotKeyType = (HotKeyTypeEnum)Enum.Parse(typeof(HotKeyTypeEnum), hotKeyTypeCode);  // 从字符串初始化热键类型
        HotKeyTypeName = HotKeyType.ToChineseName();  // 将热键类型转换为中文名称
        OnKeyPressAction = onKeyPressAction;  // 初始化按键按下时的回调动作
        IsHold = isHold;  // 初始化是否为长按状态
        SwitchHotkeyTypeEnabled = !isHold;  // 根据是否长按决定切换热键类型是否启用
    }

    public void RegisterHotKey()  // 定义注册热键的方法
    {
    }

    private void OnKeyPressed(object? sender, KeyPressedEventArgs e)  // 定义按键按下事件处理方法
    {
        OnKeyPressAction?.Invoke(sender, e);  // 如果定义了按键按下的回调动作，则调用它
    }

    private void OnKeyDown(object? sender, KeyPressedEventArgs e)  // 定义按键按下事件处理方法
    {
        OnKeyDownAction?.Invoke(sender, e);  // 如果定义了按键按下的回调动作，则调用它
    }

    private void OnKeyUp(object? sender, KeyPressedEventArgs e)  // 定义按键抬起事件处理方法
    {
        OnKeyUpAction?.Invoke(sender, e);  // 如果定义了按键抬起的回调动作，则调用它
    }

    public void UnRegisterHotKey()  // 定义注销热键的方法
    {
        GlobalRegisterHook?.Dispose();  // 释放全局热键钩子资源
        MouseMonitorHook?.Dispose();  // 释放鼠标监听钩子资源
        KeyboardMonitorHook?.Dispose();  // 释放键盘监听钩子资源
    }

    [RelayCommand]  // 定义命令
    public void OnSwitchHotKeyType()  // 定义切换热键类型的方法
    {
        HotKeyType = HotKeyType == HotKeyTypeEnum.GlobalRegister ? HotKeyTypeEnum.KeyboardMonitor : HotKeyTypeEnum.GlobalRegister;  // 切换热键类型
        HotKeyTypeName = HotKeyType.ToChineseName();  // 更新热键类型名称
    }
}
```