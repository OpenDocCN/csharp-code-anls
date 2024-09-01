# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogFolderChangeEventArgs.cs`

```cs
# 引入用于组件和控件事件处理的命名空间
using System.ComponentModel;

# 定义一个命名空间，用于组织和管理相关的代码
namespace MicaSetup.Shell.Dialogs;

# 定义一个类，继承自 CancelEventArgs，用于处理文件对话框文件夹更改事件
public class CommonFileDialogFolderChangeEventArgs : CancelEventArgs
{
    # 定义构造函数，用于初始化 Folder 属性
    public CommonFileDialogFolderChangeEventArgs(string folder) => Folder = folder;

    # 定义一个属性，用于获取或设置当前选择的文件夹路径
    public string Folder { get; set; }
}
```