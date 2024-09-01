# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogControl.cs`

```cs
namespace MicaSetup.Shell.Dialogs;

#pragma warning disable CS8618
# 禁用关于可能未初始化的引用类型变量的警告

public abstract class CommonFileDialogControl : DialogControl
{
    # 定义一个私有字段来表示控件是否启用
    private bool enabled = true;

    # 定义一个私有字段来表示控件是否已被添加
    private bool isAdded;

    # 定义一个私有字段来存储控件的文本值
    private string textValue;

    # 定义一个私有字段来表示控件是否可见
    private bool visible = true;

    # 默认构造函数
    protected CommonFileDialogControl()
    {
    }

    # 带有文本值的构造函数，调用基类构造函数并初始化文本值
    protected CommonFileDialogControl(string text)
        : base() => textValue = text;

    # 带有名称和文本值的构造函数，调用基类构造函数并初始化文本值
    protected CommonFileDialogControl(string name, string text)
        : base(name) => textValue = text;

    # 公共属性，表示控件是否启用
    public bool Enabled
    {
        get => enabled; # 获取控件是否启用的值
        set
        {
            if (value == enabled) { return; } # 如果新值与当前值相同，则不执行任何操作

            enabled = value; # 更新启用状态
            ApplyPropertyChange("Enabled"); # 应用属性更改
        }
    }

    # 虚拟公共属性，表示控件的文本值
    public virtual string Text
    {
        get => textValue; # 获取控件的文本值
        set
        {
            if (value != textValue) # 如果新值与当前值不同
            {
                textValue = value; # 更新文本值
                ApplyPropertyChange("Text"); # 应用属性更改
            }
        }
    }

    # 公共属性，表示控件是否可见
    public bool Visible
    {
        get => visible; # 获取控件是否可见的值
        set
        {
            if (value == visible) { return; } # 如果新值与当前值相同，则不执行任何操作

            visible = value; # 更新可见状态
            ApplyPropertyChange("Visible"); # 应用属性更改
        }
    }

    # 内部属性，表示控件是否已被添加
    internal bool IsAdded
    {
        get => isAdded; # 获取控件是否已被添加的值
        set => isAdded = value; # 设置控件是否已被添加的值
    }

    # 内部抽象方法，必须由派生类实现，用于将控件附加到文件对话框自定义接口
    internal abstract void Attach(IFileDialogCustomize dialog);

    # 内部虚拟方法，更新控件的启用和可见属性
    internal virtual void SyncUnmanagedProperties()
    {
        ApplyPropertyChange("Enabled"); # 应用启用状态属性更改
        ApplyPropertyChange("Visible"); # 应用可见状态属性更改
    }
}
```