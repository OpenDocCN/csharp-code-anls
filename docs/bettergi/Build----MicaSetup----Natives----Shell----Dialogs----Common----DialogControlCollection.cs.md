# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\DialogControlCollection.cs`

```cs
using System;
using System.Collections.ObjectModel;
using System.Linq;

namespace MicaSetup.Shell.Dialogs;

// 定义一个对话框控件集合类，继承自 Collection<T>，T 是 DialogControl 类型
public sealed class DialogControlCollection<T> : Collection<T> where T : DialogControl
{
    // 存储对话框控件宿主的接口实例
    private readonly IDialogControlHost hostingDialog;

    // 构造函数，初始化控件宿主
    internal DialogControlCollection(IDialogControlHost host) => hostingDialog = host;

    // 根据控件名称索引控件
    public T this[string name]
    {
        get
        {
            // 如果名称为空或null，抛出参数异常
            if (string.IsNullOrEmpty(name))
            {
                throw new ArgumentException(LocalizedMessages.DialogCollectionControlNameNull, "name");
            }

            // 返回与指定名称匹配的第一个控件，若无则返回默认值
            return Items.FirstOrDefault(x => x.Name == name);
        }
    }

    // 根据控件ID获取控件
    internal DialogControl GetControlbyId(int id) => Items.FirstOrDefault(x => x.Id == id);

    // 插入控件到集合中
    protected override void InsertItem(int index, T control)
    {
        // 如果集合中已存在相同控件，抛出无效操作异常
        if (Items.Contains(control))
        {
            throw new InvalidOperationException(LocalizedMessages.DialogCollectionCannotHaveDuplicateNames);
        }
        // 如果控件已被其它对话框托管，抛出无效操作异常
        if (control.HostingDialog != null)
        {
            throw new InvalidOperationException(LocalizedMessages.DialogCollectionControlAlreadyHosted);
        }
        // 如果不允许修改对话框集合，抛出无效操作异常
        if (!hostingDialog.IsCollectionChangeAllowed())
        {
            throw new InvalidOperationException(LocalizedMessages.DialogCollectionModifyShowingDialog);
        }

        // 设置控件的宿主为当前对话框控件宿主
        control.HostingDialog = hostingDialog;
        // 调用基类方法插入控件
        base.InsertItem(index, control);

        // 应用集合变更
        hostingDialog.ApplyCollectionChanged();
    }

    // 从集合中移除控件
    protected override void RemoveItem(int index)
    {
        // 如果不允许修改对话框集合，抛出无效操作异常
        if (!hostingDialog.IsCollectionChangeAllowed())
        {
            throw new InvalidOperationException(LocalizedMessages.DialogCollectionModifyShowingDialog);
        }

        // 获取将要移除的控件
        var control = (DialogControl)Items[index];

        // 清除控件的宿主引用
        control.HostingDialog = null!;
        // 调用基类方法移除控件
        base.RemoveItem(index);

        // 应用集合变更
        hostingDialog.ApplyCollectionChanged();
    }
}
```