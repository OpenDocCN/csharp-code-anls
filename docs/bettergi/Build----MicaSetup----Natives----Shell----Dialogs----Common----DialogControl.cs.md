# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\DialogControl.cs`

```cs
# 定义一个抽象的对话框控件类
public abstract class DialogControl
{
    # 控件的名称，初始值为 null
    private string name = null!;

    # 无参数的构造函数
    protected DialogControl()
    {
    }

    # 带名称参数的构造函数，调用无参数构造函数并设置控件名称
    protected DialogControl(string name) : this() => Name = name;

    # 控件宿主的对话框
    public IDialogControlHost HostingDialog { get; set; } = null!;

    # 控件的唯一标识符，仅有 getter
    public int Id { get; private set; }

    # 控件的名称属性，带有 setter 和 getter
    public string Name
    {
        get => name;
        set
        {
            # 如果名称为空或 null，抛出参数异常
            if (string.IsNullOrEmpty(value))
            {
                throw new ArgumentException(LocalizedMessages.DialogControlNameCannotBeEmpty);
            }

            # 如果名称已设置，且尝试更改名称，抛出操作异常
            if (!string.IsNullOrEmpty(name))
            {
                throw new InvalidOperationException(LocalizedMessages.DialogControlsCannotBeRenamed);
            }

            # 设置控件名称
            name = value;
        }
    }

    # 重写 Equals 方法，比较控件 ID 是否相等
    public override bool Equals(object obj)
    {
        if (obj is DialogControl control)
            return (Id == control.Id);

        return false;
    }

    # 重写 GetHashCode 方法，如果名称为 null，则使用 ToString() 的哈希码，否则使用名称的哈希码
    public override int GetHashCode()
    {
        if (Name == null)
        {
            return ToString().GetHashCode();
        }

        return Name.GetHashCode();
    }

    # 处理属性更改，断言属性名称不为空，并通知宿主对话框属性已更改
    protected void ApplyPropertyChange(string propName)
    {
        Debug.Assert(!string.IsNullOrEmpty(propName), "Property changed was not specified");
        HostingDialog?.ApplyControlPropertyChange(propName, this);
    }

    # 检查属性更改是否被允许，断言属性名称不为空，并询问宿主对话框是否允许更改
    protected void CheckPropertyChangeAllowed(string propName)
    {
        Debug.Assert(!string.IsNullOrEmpty(propName), "Property to change was not specified");
        HostingDialog?.IsControlPropertyChangeAllowed(propName, this);
    }
}
```