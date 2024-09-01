# `.\better-genshin-impact\Fischless.WindowsInput\IInputDeviceStateAdaptor.cs`

```cs
# 定义一个接口，处理输入设备状态的适配器
public interface IInputDeviceStateAdaptor
{
    # 判断指定虚拟键码是否被按下
    public bool IsKeyDown(User32.VK keyCode);

    # 判断指定虚拟键码是否被释放
    public bool IsKeyUp(User32.VK keyCode);

    # 判断指定虚拟键码是否在硬件层面被按下
    public bool IsHardwareKeyDown(User32.VK keyCode);

    # 判断指定虚拟键码是否在硬件层面被释放
    public bool IsHardwareKeyUp(User32.VK keyCode);

    # 判断指定虚拟键码是否是切换键并且效果生效
    public bool IsTogglingKeyInEffect(User32.VK keyCode);
}
```