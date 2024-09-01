# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogControlCollection.cs`

```cs
# 定义一个泛型类 CommonFileDialogControlCollection 继承自 Collection<T>
# 其中 T 必须是 DialogControl 的子类
public sealed class CommonFileDialogControlCollection<T> : Collection<T> where T : DialogControl
{
    # 存储对话框控制主机的引用
    private readonly IDialogControlHost hostingDialog;

    # 构造函数，初始化 hostingDialog
    internal CommonFileDialogControlCollection(IDialogControlHost host) => hostingDialog = host;

    # 根据名称获取控制项
    public T this[string name]
    {
        get
        {
            # 如果名称为空或 null，抛出异常
            if (string.IsNullOrEmpty(name))
            {
                throw new ArgumentException(LocalizedMessages.DialogControlCollectionEmptyName, "name");
            }

            # 遍历当前集合中的所有控制项
            foreach (var control in base.Items)
            {
                CommonFileDialogGroupBox groupBox;
                # 如果控制项的名称匹配，返回该控制项
                if (control.Name == name)
                {
                    return control;
                }
                # 如果控制项是 CommonFileDialogGroupBox 类型，进一步查找
                else if ((groupBox = (control as CommonFileDialogGroupBox)!) != null)
                {
                    # 遍历组框中的控制项，查找匹配的名称
                    foreach (T subControl in groupBox.Items)
                    {
                        if (subControl.Name == name) { return subControl; }
                    }
                }
            }
            # 如果没有找到，返回 null
            return null!;
        }
    }

    # 根据 ID 获取控制项
    internal DialogControl GetControlbyId(int id) => GetSubControlbyId(Items.Cast<DialogControl>(), id);

    # 从控制项集合中递归查找指定 ID 的控制项
    internal DialogControl GetSubControlbyId(IEnumerable<DialogControl> controlCollection, int id)
    {
        # 如果控制项集合为 null，返回 null
        if (controlCollection == null) { return null!; }

        # 遍历控制项集合
        foreach (var control in controlCollection)
        {
            # 如果控制项的 ID 匹配，返回该控制项
            if (control.Id == id) { return control; }

            # 如果控制项是 CommonFileDialogGroupBox 类型，递归查找子控制项
            var groupBox = control as CommonFileDialogGroupBox;
            if (groupBox != null)
            {
                var temp = GetSubControlbyId(groupBox.Items, id);
                if (temp != null) { return temp; }
            }
        }

        # 如果没有找到，返回 null
        return null!;
    }

    # 重写 InsertItem 方法，以插入新的控制项
    protected override void InsertItem(int index, T control)
    {
        # 如果集合中已存在相同的控制项，抛出异常
        if (Items.Contains(control))
        {
            throw new InvalidOperationException(
                LocalizedMessages.DialogControlCollectionMoreThanOneControl);
        }
        # 如果控制项已经有主机对话框，抛出异常
        if (control.HostingDialog != null)
        {
            throw new InvalidOperationException(
                LocalizedMessages.DialogControlCollectionRemoveControlFirst);
        }
        # 如果主机对话框不允许集合修改，抛出异常
        if (!hostingDialog.IsCollectionChangeAllowed())
        {
            throw new InvalidOperationException(
                LocalizedMessages.DialogControlCollectionModifyingControls);
        }
        # 如果控制项是 CommonFileDialogMenuItem 类型，抛出异常
        if (control is CommonFileDialogMenuItem)
        {
            throw new InvalidOperationException(
                LocalizedMessages.DialogControlCollectionMenuItemControlsCannotBeAdded);
        }

        # 设置控制项的主机对话框为当前对话框
        control.HostingDialog = hostingDialog;
        # 调用基类方法插入控制项
        base.InsertItem(index, control);

        # 通知主机对话框集合已更改
        hostingDialog.ApplyCollectionChanged();
    }
    // 重写 RemoveItem 方法，在尝试移除项目时抛出不支持的操作异常
    protected override void RemoveItem(int index) => throw new NotSupportedException(LocalizedMessages.DialogControlCollectionCannotRemoveControls);
# 代码块的结束符号
}
```