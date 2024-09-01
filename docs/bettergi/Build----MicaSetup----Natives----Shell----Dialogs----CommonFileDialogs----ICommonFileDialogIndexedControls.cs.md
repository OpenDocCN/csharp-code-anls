# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\ICommonFileDialogIndexedControls.cs`

```cs
# 定义一个内部接口，描述一个通用文件对话框中的控件
internal interface ICommonFileDialogIndexedControls
{
    # 声明一个事件，当选中的索引发生变化时触发
    event EventHandler SelectedIndexChanged;

    # 属性：获取或设置当前选中的索引
    int SelectedIndex { get; set; }

    # 方法：触发 SelectedIndexChanged 事件
    void RaiseSelectedIndexChangedEvent();
}
```