# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\KnownFolders\RedirectionCapabilities.cs`

```cs
namespace MicaSetup.Shell.Dialogs;  // 定义一个命名空间，用于组织相关的类和枚举

public enum RedirectionCapability  // 定义一个公共的枚举类型，表示重定向能力的不同选项
{
    None = 0x00,  // 无重定向能力
    AllowAll = 0xff,  // 允许所有的重定向
    Redirectable = 0x1,  // 允许重定向
    DenyAll = 0xfff00,  // 拒绝所有重定向
    DenyPolicyRedirected = 0x100,  // 拒绝策略重定向
    DenyPolicy = 0x200,  // 拒绝策略
    DenyPermissions = 0x400,  // 拒绝权限
}
```