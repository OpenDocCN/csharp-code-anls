# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Taskbar\TaskbarCOMInterfaces.cs`

```cs
# 引入系统和运行时互操作性命名空间
﻿using System;
using System.Runtime.InteropServices;

# 定义 MicaSetup.Shell.Dialogs 命名空间
namespace MicaSetup.Shell.Dialogs;

# 标记此接口为 COM 导入接口，并定义其 GUID 和接口类型
[ComImportAttribute()]
[GuidAttribute("6332DEBF-87B5-4670-90C0-5E57B408A49E")]
[InterfaceTypeAttribute(ComInterfaceType.InterfaceIsIUnknown)]
internal interface ICustomDestinationList
{
    # 设置应用程序 ID
    void SetAppID(
        [MarshalAs(UnmanagedType.LPWStr)] string pszAppID);

    # 开始创建列表，获取最大插槽数和接口对象
    [PreserveSig]
    HResult BeginList(
        out uint cMaxSlots,
        ref Guid riid,
        [Out(), MarshalAs(UnmanagedType.Interface)] out object ppvObject);

    # 向列表中添加类别
    [PreserveSig]
    HResult AppendCategory(
        [MarshalAs(UnmanagedType.LPWStr)] string pszCategory,
        [MarshalAs(UnmanagedType.Interface)] IObjectArray poa);

    # 向列表中添加已知类别
    void AppendKnownCategory(
        [MarshalAs(UnmanagedType.I4)] KnownDestinationCategory category);

    # 向列表中添加用户任务
    [PreserveSig]
    HResult AddUserTasks(
        [MarshalAs(UnmanagedType.Interface)] IObjectArray poa);

    # 提交列表
    void CommitList();

    # 获取已删除的目标
    void GetRemovedDestinations(
        ref Guid riid,
        [Out(), MarshalAs(UnmanagedType.Interface)] out object ppvObject);

    # 删除列表
    void DeleteList(
        [MarshalAs(UnmanagedType.LPWStr)] string pszAppID);

    # 中止列表操作
    void AbortList();
}

# 定义另一个 COM 导入接口，用于表示对象数组
[ComImportAttribute()]
[GuidAttribute("92CA9DCD-5622-4BBA-A805-5E9F541BD8C9")]
[InterfaceTypeAttribute(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IObjectArray
{
    # 获取对象数组中的对象数量
    void GetCount(out uint cObjects);

    # 获取指定索引的对象
    void GetAt(
        uint iIndex,
        ref Guid riid,
        [Out(), MarshalAs(UnmanagedType.Interface)] out object ppvObject);
}

# 定义 COM 导入接口，用于表示对象集合
[ComImportAttribute()]
[GuidAttribute("5632B1A4-E38A-400A-928A-D4CD63230295")]
[InterfaceTypeAttribute(ComInterfaceType.InterfaceIsIUnknown)]
internal interface IObjectCollection
{
    # 获取集合中的对象数量
    [PreserveSig]
    void GetCount(out uint cObjects);

    # 获取指定索引的对象
    [PreserveSig]
    void GetAt(
        uint iIndex,
        ref Guid riid,
        [Out(), MarshalAs(UnmanagedType.Interface)] out object ppvObject);

    # 向集合中添加对象
    void AddObject(
        [MarshalAs(UnmanagedType.Interface)] object pvObject);

    # 从对象数组中添加对象
    void AddFromArray(
        [MarshalAs(UnmanagedType.Interface)] IObjectArray poaSource);

    # 从集合中移除对象
    void RemoveObject(uint uiIndex);

    # 清空集合
    void Clear();
}

# 定义 COM 导入接口，用于表示任务栏列表
[ComImportAttribute()]
[GuidAttribute("c43dc798-95d1-4bea-9030-bb99e2983a1a")]
[InterfaceTypeAttribute(ComInterfaceType.InterfaceIsIUnknown)]
internal interface ITaskbarList4
{
    # 初始化任务栏列表
    [PreserveSig]
    void HrInit();

    # 向任务栏添加标签
    [PreserveSig]
    void AddTab(nint hwnd);

    # 从任务栏删除标签
    [PreserveSig]
    void DeleteTab(nint hwnd);

    # 激活任务栏标签
    [PreserveSig]
    void ActivateTab(nint hwnd);

    # 设置活动的备用窗口
    [PreserveSig]
    void SetActiveAlt(nint hwnd);

    # 标记全屏窗口
    [PreserveSig]
    void MarkFullscreenWindow(
        nint hwnd,
        [MarshalAs(UnmanagedType.Bool)] bool fFullscreen);

    # 设置任务栏进度值
    [PreserveSig]
    void SetProgressValue(nint hwnd, ulong ullCompleted, ulong ullTotal);

    # 设置任务栏进度状态
    [PreserveSig]
    void SetProgressState(nint hwnd, TaskbarProgressBarStatus tbpFlags);

    # 注册任务栏标签和多文档界面窗口
    [PreserveSig]
    void RegisterTab(nint hwndTab, nint hwndMDI);

    [PreserveSig]
    // 注销一个选项卡
    void UnregisterTab(nint hwndTab);

    // 设置选项卡的顺序，指定插入前的位置
    [PreserveSig]
    void SetTabOrder(nint hwndTab, nint hwndInsertBefore);

    // 激活一个选项卡，指定插入前的位置，并使用保留的参数
    [PreserveSig]
    void SetTabActive(nint hwndTab, nint hwndInsertBefore, uint dwReserved);

    // 添加缩略图条按钮，并返回操作结果
    [PreserveSig]
    HResult ThumbBarAddButtons(
        nint hwnd,
        uint cButtons,
        [MarshalAs(UnmanagedType.LPArray)] ThumbButton[] pButtons);

    // 更新缩略图条按钮，并返回操作结果
    [PreserveSig]
    HResult ThumbBarUpdateButtons(
        nint hwnd,
        uint cButtons,
        [MarshalAs(UnmanagedType.LPArray)] ThumbButton[] pButtons);

    // 设置缩略图条的图像列表
    [PreserveSig]
    void ThumbBarSetImageList(nint hwnd, nint himl);

    // 设置覆盖图标及其描述
    [PreserveSig]
    void SetOverlayIcon(
      nint hwnd,
      nint hIcon,
      [MarshalAs(UnmanagedType.LPWStr)] string pszDescription);

    // 设置缩略图的提示文本
    [PreserveSig]
    void SetThumbnailTooltip(
        nint hwnd,
        [MarshalAs(UnmanagedType.LPWStr)] string pszTip);

    // 设置缩略图的剪裁区域
    [PreserveSig]
    void SetThumbnailClip(
        nint hwnd,
        nint prcClip);

    // 设置选项卡属性
    void SetTabProperties(nint hwndTab, SetTabPropertiesOption stpFlags);
# 定义一个空的类，通常是作为接口实现的占位符
}

# 声明一个具有唯一 GUID 的 COM 类，并且不使用默认的接口
[GuidAttribute("77F10CF0-3DB5-4966-B520-B7C54FD35ED6")]
# 指定类接口不自动生成
[ClassInterfaceAttribute(ClassInterfaceType.None)]
# 标记该类为 COM 组件
[ComImportAttribute()]
internal class CDestinationList { }

# 声明一个具有唯一 GUID 的 COM 类，并且不使用默认的接口
[GuidAttribute("2D3468C1-36A7-43B6-AC24-D3F02FD9607A")]
# 指定类接口不自动生成
[ClassInterfaceAttribute(ClassInterfaceType.None)]
# 标记该类为 COM 组件
[ComImportAttribute()]
internal class CEnumerableObjectCollection { }

# 声明一个具有唯一 GUID 的 COM 类，并且不使用默认的接口
[GuidAttribute("56FDF344-FD6D-11d0-958A-006097C9A090")]
# 指定类接口不自动生成
[ClassInterfaceAttribute(ClassInterfaceType.None)]
# 标记该类为 COM 组件
[ComImportAttribute()]
internal class CTaskbarList { }
```