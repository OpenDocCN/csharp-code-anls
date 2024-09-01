# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Interop\CoreErrorHelper.cs`

```cs
# 定义 HResult 枚举类型，表示各种操作结果的 HRESULT 错误代码
public enum HResult
{
    # 操作成功
    Ok = 0x0000,
    # 操作失败
    False = 0x0001,
    # 参数无效
    InvalidArguments = unchecked((int)0x80070057),
    # 内存不足
    OutOfMemory = unchecked((int)0x8007000E),
    # 接口不存在
    NoInterface = unchecked((int)0x80004002),
    # 操作失败
    Fail = unchecked((int)0x80004005),
    # 元素未找到
    ElementNotFound = unchecked((int)0x80070490),
    # 类型元素未找到
    TypeElementNotFound = unchecked((int)0x8002802B),
    # 对象不存在
    NoObject = unchecked((int)0x800401E5),
    # 操作被取消 (Win32 错误代码)
    Win32ErrorCanceled = 1223,
    # 操作被取消
    Canceled = unchecked((int)0x800704C7),
    # 资源正被使用
    ResourceInUse = unchecked((int)0x800700AA),
    # 访问被拒绝
    AccessDenied = unchecked((int)0x80030005)
}

# 内部类，用于处理错误相关的帮助方法
internal static class CoreErrorHelper
{
    # 忽略的结果常量（表示成功）
    public const int Ignored = (int)HResult.Ok;

    # Win32 错误代码的设施代码
    private const int FacilityWin32 = 7;

    # 判断 HRESULT 结果是否失败
    public static bool Failed(HResult result) => !Succeeded(result);

    # 判断整数结果是否失败
    public static bool Failed(int result) => !Succeeded(result);

    # 根据 Win32 错误代码生成对应的 HRESULT 结果
    public static int HResultFromWin32(int win32ErrorCode)
    {
        if (win32ErrorCode > 0)
        {
            # 生成 HRESULT 结果
            win32ErrorCode =
                (int)(((uint)win32ErrorCode & 0x0000FFFF) | (FacilityWin32 << 16) | 0x80000000);
        }
        return win32ErrorCode;
    }

    # 判断结果是否与指定的 Win32 错误代码匹配
    public static bool Matches(int result, int win32ErrorCode) => (result == HResultFromWin32(win32ErrorCode));

    # 判断结果是否成功
    public static bool Succeeded(int result) => result >= 0;

    # 判断 HRESULT 结果是否成功
    public static bool Succeeded(HResult result) => Succeeded((int)result);
}
```