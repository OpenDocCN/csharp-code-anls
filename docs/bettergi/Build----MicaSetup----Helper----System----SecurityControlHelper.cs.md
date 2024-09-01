# `.\better-genshin-impact\Build\MicaSetup\Helper\System\SecurityControlHelper.cs`

```cs
﻿using System;
using System.IO;
using System.Security.AccessControl;
using System.Windows;

namespace MicaSetup.Helper;

public static class SecurityControlHelper
{
    // 允许完全文件安全
    public static void AllowFullFileSecurity(string filePath)
    {
        // 如果没有管理员权限，则退出方法
        if (!RuntimeHelper.IsElevated)
        {
            return;
        }

        try
        {
            // 创建文件信息对象
            FileInfo fileInfo = new(filePath);
            // 获取文件的访问控制信息
            FileSecurity fileSecurity = fileInfo.GetAccessControl();
            // 添加 "Everyone" 组的完全控制权限
            fileSecurity.AddAccessRule(new FileSystemAccessRule("Everyone", FileSystemRights.FullControl, AccessControlType.Allow));
            // 添加 "Users" 组的完全控制权限
            fileSecurity.AddAccessRule(new FileSystemAccessRule("Users", FileSystemRights.FullControl, AccessControlType.Allow));
            // 设置文件的访问控制
            fileInfo.SetAccessControl(fileSecurity);
        }
        catch (Exception e)
        {
            // 记录错误信息
            Logger.Error(e);
            // 弹出错误信息对话框
            MessageBox.Show("Allow Full File Security Error" + e.ToString());
        }
    }

    // 允许完全文件夹安全
    public static void AllowFullFolderSecurity(string dirPath)
    {
        // 如果没有管理员权限，则退出方法
        if (!RuntimeHelper.IsElevated)
        {
            return;
        }

        try
        {
            // 创建目录信息对象
            DirectoryInfo dir = new(dirPath);
            // 获取目录的访问控制信息
            DirectorySecurity dirSecurity = dir.GetAccessControl(AccessControlSections.All);
            // 定义继承标志，允许对象和容器继承
            InheritanceFlags inherits = InheritanceFlags.ContainerInherit | InheritanceFlags.ObjectInherit;
            // 创建 "Everyone" 组的完全控制权限规则
            FileSystemAccessRule everyoneFileSystemAccessRule = new("Everyone", FileSystemRights.FullControl, inherits, PropagationFlags.None, AccessControlType.Allow);
            // 创建 "Users" 组的完全控制权限规则
            FileSystemAccessRule usersFileSystemAccessRule = new("Users", FileSystemRights.FullControl, inherits, PropagationFlags.None, AccessControlType.Allow);
            // 添加 "Everyone" 组的访问规则
            dirSecurity.ModifyAccessRule(AccessControlModification.Add, everyoneFileSystemAccessRule, out _);
            // 添加 "Users" 组的访问规则
            dirSecurity.ModifyAccessRule(AccessControlModification.Add, usersFileSystemAccessRule, out _);
            // 设置目录的访问控制
            dir.SetAccessControl(dirSecurity);
        }
        catch (Exception e)
        {
            // 记录错误信息
            Logger.Error(e);
            // 弹出错误信息对话框
            MessageBox.Show("Allow Full Folder Security Error" + e.ToString());
        }
    }
}
```