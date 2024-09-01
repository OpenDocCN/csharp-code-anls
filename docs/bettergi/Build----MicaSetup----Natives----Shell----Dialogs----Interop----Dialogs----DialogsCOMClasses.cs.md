# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\Dialogs\DialogsCOMClasses.cs`

```cs
# 定义一个内部接口，不包含方法或属性
internal interface NativeCommonFileDialog
{
}

# 标记该接口为 COM 进口接口，指定 GUID 和 CoClass
[ComImport,
Guid(ShellIIDGuid.IFileOpenDialog),
CoClass(typeof(FileOpenDialogRCW))]
internal interface NativeFileOpenDialog : IFileOpenDialog
{
}

# 标记该接口为 COM 进口接口，指定 GUID 和 CoClass
[ComImport,
Guid(ShellIIDGuid.IFileSaveDialog),
CoClass(typeof(FileSaveDialogRCW))]
internal interface NativeFileSaveDialog : IFileSaveDialog
{
}

# 标记该类为 COM 进口类，指定 ClassInterface、TypeLibType 和 GUID
[ComImport,
ClassInterface(ClassInterfaceType.None),
TypeLibType(TypeLibTypeFlags.FCanCreate),
Guid(ShellCLSIDGuid.FileOpenDialog)]
internal class FileOpenDialogRCW
{
}

# 标记该类为 COM 进口类，指定 ClassInterface、TypeLibType 和 GUID
[ComImport,
ClassInterface(ClassInterfaceType.None),
TypeLibType(TypeLibTypeFlags.FCanCreate),
Guid(ShellCLSIDGuid.FileSaveDialog)]
internal class FileSaveDialogRCW
{
}
```