# `.\better-genshin-impact\Build\MicaSetup\Natives\UxTheme.cs`

```cs
﻿using System;
using System.Runtime.InteropServices;

namespace MicaSetup.Natives;

public static class UxTheme
{
    # 定义一个外部方法，允许设置窗口主题属性
    [DllImport(Lib.UxTheme, SetLastError = false, ExactSpelling = true)]
    public static extern int SetWindowThemeAttribute(nint hwnd, WINDOWTHEMEATTRIBUTETYPE eAttribute, in WTA_OPTIONS pvAttribute, uint cbAttribute);

    public enum WINDOWTHEMEATTRIBUTETYPE
    {
        # 确定窗口的非客户端区域主题属性
        WTA_NONCLIENT = 1,
    }

    [Flags]
    public enum ThemeDialogTextureFlags
    {
        /// <summary>禁用背景纹理。</summary>
        ETDT_DISABLE = 0x00000001,

        /// <summary>启用对话框窗口的背景纹理。纹理由视觉样式定义。</summary>
        ETDT_ENABLE = 0x00000002,

        /// <summary>使用 Tab 控件的纹理作为对话框窗口的背景纹理。</summary>
        ETDT_USETABTEXTURE = 0x00000004,

        /// <summary>
        /// 启用对话框窗口背景纹理。纹理是由视觉样式定义的 Tab 控件纹理。此标志等效于 (ETDT_ENABLE | ETDT_USETABTEXTURE)。
        /// </summary>
        ETDT_ENABLETAB = (ETDT_ENABLE | ETDT_USETABTEXTURE),

        /// <summary>使用 Aero 向导纹理作为对话框窗口的背景纹理。</summary>
        ETDT_USEAEROWIZARDTABTEXTURE = 0x00000008,

        /// <summary>ETDT_ENABLE | ETDT_USEAEROWIZARDTABTEXTURE。</summary>
        ETDT_ENABLEAEROWIZARDTAB = (ETDT_ENABLE | ETDT_USEAEROWIZARDTABTEXTURE),

        /// <summary>ETDT_DISABLE | ETDT_ENABLE | ETDT_USETABTEXTURE | ETDT_USEAEROWIZARDTABTEXTURE。</summary>
        ETDT_VALIDBITS = (ETDT_DISABLE | ETDT_ENABLE | ETDT_USETABTEXTURE | ETDT_USEAEROWIZARDTABTEXTURE),
    }

    [Flags]
    public enum WTNCA
    {
        /// <summary>防止绘制窗口标题栏。</summary>
        WTNCA_NODRAWCAPTION = 0x00000001,

        /// <summary>防止绘制系统图标。</summary>
        WTNCA_NODRAWICON = 0x00000002,

        /// <summary>防止系统图标菜单出现。</summary>
        WTNCA_NOSYSMENU = 0x00000004,

        /// <summary>即使在从右到左 (RTL) 布局中也防止镜像问号。</summary>
        WTNCA_NOMIRRORHELP = 0x00000008
    }

    public struct WTA_OPTIONS
    {
        # 窗口主题属性标志
        public WTNCA Flags;
        # 用于指示哪些属性有效的掩码
        public uint Mask;
    }
}
```