# `.\better-genshin-impact\Fischless.GameCapture\DwmSharedSurface\SharedSurfaceCapture.cs`

```cs
# 导入 Fischless.GameCapture.DwmSharedSurface.Helpers 命名空间中的帮助类
﻿using Fischless.GameCapture.DwmSharedSurface.Helpers;
# 导入 Fischless.GameCapture.Graphics.Helpers 命名空间中的帮助类
using Fischless.GameCapture.Graphics.Helpers;
# 导入 SharpDX.Direct3D11 命名空间中的 Direct3D 11 相关类
using SharpDX.Direct3D11;
# 导入 SharpDX.DXGI 命名空间中的 DXGI 相关类
using SharpDX.DXGI;
# 导入 System.Diagnostics 命名空间中的诊断类
using System.Diagnostics;
# 导入 Vanara.PInvoke 命名空间中的 PInvoke 方法
using Vanara.PInvoke;
# 使用 SharpDX.Direct3D11 命名空间中的 Device 类的别名
using Device = SharpDX.Direct3D11.Device;

# 定义 Fischless.GameCapture.DwmSharedSurface 命名空间
namespace Fischless.GameCapture.DwmSharedSurface
{
    # 定义 SharedSurfaceCapture 类，继承自 IGameCapture 接口
    public class SharedSurfaceCapture : IGameCapture
    {
    }
}
```