# `.\better-genshin-impact\Build\MicaSetup\Natives\NativeEnums.cs`

```cs
# 定义与窗口的长整型属性相关的常量
public enum WindowLongFlags
{
    # 窗口扩展样式
    GWL_EXSTYLE = -20,
    # 窗口实例句柄（也可以通过GWLP_HINSTANCE获取）
    GWL_HINSTANCE = -6,
    # 窗口实例句柄（与GWL_HINSTANCE相同）
    GWLP_HINSTANCE = -6,
    # 窗口父窗口句柄
    GWL_HWNDPARENT = -8,
    # 窗口标识符
    GWL_ID = -12,
    # 窗口标识符（与GWL_ID相同）
    GWLP_ID = -12,
    # 窗口样式
    GWL_STYLE = -16,
    # 用户数据
    GWL_USERDATA = -21,
    # 用户数据（与GWL_USERDATA相同）
    GWLP_USERDATA = -21,
    # 窗口过程地址
    GWL_WNDPROC = -4,
    # 窗口过程地址（与GWL_WNDPROC相同）
    GWLP_WNDPROC = -4,
    # 额外用户数据
    DWLP_USER = 8,
    # 消息结果
    DWLP_MSGRESULT = 0,
    # 对话框过程地址
    DWLP_DLGPROC = 4,
    # 额外用户数据（与DWLP_USER相同）
    DWL_USER = 8,
    # 消息结果（与DWLP_MSGRESULT相同）
    DWL_MSGRESULT = 0,
    # 对话框过程地址（与DWLP_DLGPROC相同）
    DWL_DLGPROC = 4,
}

[Flags]
public enum WindowStyles : uint
{
    # 窗口边框
    WS_BORDER = 0x800000u,
    # 窗口标题栏
    WS_CAPTION = 0xC00000u,
    # 子窗口
    WS_CHILD = 0x40000000u,
    # 剪切子窗口
    WS_CLIPCHILDREN = 0x2000000u,
    # 剪切兄弟窗口
    WS_CLIPSIBLINGS = 0x4000000u,
    # 禁用窗口
    WS_DISABLED = 0x8000000u,
    # 对话框边框
    WS_DLGFRAME = 0x400000u,
    # 组窗口
    WS_GROUP = 0x20000u,
    # 水平滚动条
    WS_HSCROLL = 0x100000u,
    # 最大化窗口
    WS_MAXIMIZE = 0x1000000u,
    # 最大化按钮
    WS_MAXIMIZEBOX = 0x10000u,
    # 最小化窗口
    WS_MINIMIZE = 0x20000000u,
    # 最小化按钮
    WS_MINIMIZEBOX = 0x20000u,
    # 重叠窗口（默认）
    WS_OVERLAPPED = 0x0u,
    # 重叠窗口及其边框
    WS_OVERLAPPEDWINDOW = 0xCF0000u,
    # 弹出窗口
    WS_POPUP = 0x80000000u,
    # 弹出窗口及其边框
    WS_POPUPWINDOW = 0x80880000u,
    # 窗口厚边框
    WS_THICKFRAME = 0x40000u,
    # 系统菜单
    WS_SYSMENU = 0x80000u,
    # 选项卡停止
    WS_TABSTOP = 0x10000u,
    # 可见窗口
    WS_VISIBLE = 0x10000000u,
    # 垂直滚动条
    WS_VSCROLL = 0x200000u,
    # 磁贴窗口（与重叠窗口相同）
    WS_TILED = 0x0u,
    # 最小化窗口（与最小化相同）
    WS_ICONIC = 0x20000000u,
    # 窗口大小调整框
    WS_SIZEBOX = 0x40000u,
    # 磁贴窗口及其边框
    WS_TILEDWINDOW = 0xCF0000u,
    # 子窗口（与WS_CHILD相同）
    WS_CHILDWINDOW = 0x40000000u,
}

[Flags]
public enum DWMWINDOWATTRIBUTE
{
    # 启用非客户区渲染
    DWMWA_NCRENDERING_ENABLED = 1,
    # 非客户区渲染策略
    DWMWA_NCRENDERING_POLICY,
    # 强制禁用过渡
    DWMWA_TRANSITIONS_FORCEDISABLED,
    # 允许非客户区绘制
    DWMWA_ALLOW_NCPAINT,
    # 标题栏按钮边界
    DWMWA_CAPTION_BUTTON_BOUNDS,
    # 非客户区 RTL 布局
    DWMWA_NONCLIENT_RTL_LAYOUT,
    # 强制图标化表示
    DWMWA_FORCE_ICONIC_REPRESENTATION,
    # Flip3D 策略
    DWMWA_FLIP3D_POLICY,
    # 扩展框架边界
    DWMWA_EXTENDED_FRAME_BOUNDS,
    # 是否有图标化位图
    DWMWA_HAS_ICONIC_BITMAP,
    # 禁止 Peek 操作
    DWMWA_DISALLOW_PEEK,
    # 从 Peek 操作中排除
    DWMWA_EXCLUDED_FROM_PEEK,
    # 隐藏窗口
    DWMWA_CLOAK,
    # 窗口是否被隐藏
    DWMWA_CLOAKED,
    # 冻结表示
    DWMWA_FREEZE_REPRESENTATION,
    # 使用主机背景刷子
    DWMWA_USE_HOSTBACKDROPBRUSH,
    # 使用旧版沉浸式黑暗模式
    DWMWA_USE_IMMERSIVE_DARK_MODE_OLD = 19,
    # 使用沉浸式黑暗模式
    DWMWA_USE_IMMERSIVE_DARK_MODE = 20,
    # 窗口角落偏好
    DWMWA_WINDOW_CORNER_PREFERENCE = 33,
    # 边框颜色
    DWMWA_BORDER_COLOR,
    # 标题栏颜色
    DWMWA_CAPTION_COLOR,
    # 文字颜色
    DWMWA_TEXT_COLOR,
    # 可见框架边框厚度
    DWMWA_VISIBLE_FRAME_BORDER_THICKNESS,
    # 系统背景类型
    DWMWA_SYSTEMBACKDROP_TYPE,
    # Mica 效果
    DWMWA_MICA_EFFECT = 1029,
}

# 进程 DPI 感知级别
public enum PROCESS_DPI_AWARENESS
{
    # 不感知 DPI
    PROCESS_DPI_UNAWARE,
    # 系统 DPI 感知
    PROCESS_SYSTEM_DPI_AWARE,
    # 每监视器 DPI 感知
    PROCESS_PER_MONITOR_DPI_AWARE,
}

# 监视器标志
public enum MonitorFlags
{
    # 不使用监视器
    MONITOR_DEFAULTTONULL,
    # 默认使用主监视器
    MONITOR_DEFAULTTOPRIMARY,
    # 默认使用最近的监视器
    MONITOR_DEFAULTTONEAREST,
}

# 监视器 DPI 类型
public enum MONITOR_DPI_TYPE
{
    # 实际有效 DPI
    MDT_EFFECTIVE_DPI = 0,
    # 角度 DPI
    MDT_ANGULAR_DPI,
    # 原始 DPI
    MDT_RAW_DPI,
    # 默认 DPI 类型（有效 DPI）
    MDT_DEFAULT = MDT_EFFECTIVE_DPI,
}

[Flags]
public enum SetWindowPosFlags : uint
{
    # 异步设置窗口位置
    SWP_ASYNCWINDOWPOS = 0x4000,
    # 延迟擦除
    SWP_DEFERERASE = 0x2000,
    # 绘制窗口框架
    SWP_DRAWFRAME = 0x0020,
    # 框架已更改
    SWP_FRAMECHANGED = 0x0020,
    # 隐藏窗口
    SWP_HIDEWINDOW = 0x0080,
    # 不激活窗口
    SWP_NOACTIVATE = 0x0010,
    # 不复制位图
    SWP_NOCOPYBITS = 0x0100,
    # 不移动窗口
    SWP_NOMOVE = 0x0002,
    # 不调整窗口的顺序
    SWP_NOOWNERZORDER = 0x0200,
    # 不重绘窗口
    SWP_NOREDRAW = 0x0008,
    # 不重新定位窗口
    SWP_NOREPOSITION = 0x0200,
    # 不发送改变消息
    SWP_NOSENDCHANGING = 0x0400,
    # 不调整窗口大小
    SWP_NOSIZE = 0x0001,
    # 不改变窗口 Z 顺序
    SWP_NOZORDER = 0x0004,
    # 显示窗口
    SWP_SHOWWINDOW = 0x0040,
}
# 定义了文件移动标志的枚举
public enum MoveFileFlags
{
    # 替换现有文件
    MOVEFILE_REPLACE_EXISTING = 0x1,
    # 允许复制文件
    MOVEFILE_COPY_ALLOWED = 0x2,
    # 延迟直到重启系统时移动文件
    MOVEFILE_DELAY_UNTIL_REBOOT = 0x4,
    # 立即写入操作系统
    MOVEFILE_WRITE_THROUGH = 0x8,
    # 创建硬链接
    MOVEFILE_CREATE_HARDLINK = 0x10,
    # 如果文件不可跟踪，则移动操作失败
    MOVEFILE_FAIL_IF_NOT_TRACKABLE = 0x20,
}

# 定义了设备能力的枚举
public enum DeviceCap
{
    # 驱动版本
    DRIVERVERSION = 0,
    # 技术类型
    TECHNOLOGY = 2,
    # 水平方向尺寸
    HORZSIZE = 4,
    # 垂直方向尺寸
    VERTSIZE = 6,
    # 水平方向分辨率
    HORZRES = 8,
    # 垂直方向分辨率
    VERTRES = 10,
    # 每像素位数
    BITSPIXEL = 12,
    # 颜色平面数
    PLANES = 14,
    # 画笔数量
    NUMBRUSHES = 16,
    # 笔数量
    NUMPENS = 18,
    # 标记器数量
    NUMMARKERS = 20,
    # 字体数量
    NUMFONTS = 22,
    # 颜色数量
    NUMCOLORS = 24,
    # 设备尺寸
    PDEVICESIZE = 26,
    # 曲线功能
    CURVECAPS = 28,
    # 线条功能
    LINECAPS = 30,
    # 多边形功能
    POLYGONALCAPS = 32,
    # 文本功能
    TEXTCAPS = 34,
    # 剪辑功能
    CLIPCAPS = 36,
    # 光栅功能
    RASTERCAPS = 38,
    # X方向的长宽比
    ASPECTX = 40,
    # Y方向的长宽比
    ASPECTY = 42,
    # XY方向的长宽比
    ASPECTXY = 44,
    # 阴影混合功能
    SHADEBLENDCAPS = 45,
    # X方向的逻辑像素数
    LOGPIXELSX = 88,
    # Y方向的逻辑像素数
    LOGPIXELSY = 90,
    # 调色板大小
    SIZEPALETTE = 104,
    # 保留的数量
    NUMRESERVED = 106,
    # 颜色分辨率
    COLORRES = 108,
    # 物理宽度
    PHYSICALWIDTH = 110,
    # 物理高度
    PHYSICALHEIGHT = 111,
    # 物理X方向偏移
    PHYSICALOFFSETX = 112,
    # 物理Y方向偏移
    PHYSICALOFFSETY = 113,
    # X方向缩放因子
    SCALINGFACTORX = 114,
    # Y方向缩放因子
    SCALINGFACTORY = 115,
    # 刷新率
    VREFRESH = 116,
    # 桌面垂直分辨率
    DESKTOPVERTRES = 117,
    # 桌面水平分辨率
    DESKTOPHORZRES = 118,
    # BLT对齐方式
    BLTALIGNMENT = 119,
}

# 定义了监视器选项的枚举
public enum MonitorOptions : uint
{
    # 默认不监视任何监视器
    MONITOR_DEFAULTTONULL = 0x00000000,
    # 默认监视主监视器
    MONITOR_DEFAULTTOPRIMARY = 0x00000001,
    # 默认监视最近的监视器
    MONITOR_DEFAULTTONEAREST = 0x00000002,
}

# 定义了监视器 DPI 类型的枚举
public enum MonitorDpiType
{
    # 有效 DPI
    MDT_Effective_DPI = 0,
    # 角度 DPI
    MDT_Angular_DPI = 1,
    # 原始 DPI
    MDT_Raw_DPI = 2,
}

# 定义了共享的枚举
public enum SHARD
{
    # 应用程序 ID 信息
    SHARD_APPIDINFO = 4,
    # 应用程序 ID 信息 ID 列表
    SHARD_APPIDINFOIDLIST = 5,
    # 应用程序 ID 信息链接
    SHARD_APPIDINFOLINK = 7,
    # 链接
    SHARD_LINK = 6,
    # 路径 ANSI
    SHARD_PATHA = 2,
    # 路径 Unicode
    SHARD_PATHW = 3,
    # PIDL
    SHARD_PIDL = 1,
    # Shell 项目
    SHARD_SHELLITEM = 8,
}

# 定义了系统通知事件的枚举
[Flags]
public enum SHCNE : uint
{
    # 重命名项目
    SHCNE_RENAMEITEM = 0x00000001,
    # 创建项目
    SHCNE_CREATE = 0x00000002,
    # 删除项目
    SHCNE_DELETE = 0x00000004,
    # 创建目录
    SHCNE_MKDIR = 0x00000008,
    # 删除目录
    SHCNE_RMDIR = 0x00000010,
    # 媒体插入
    SHCNE_MEDIAINSERTED = 0x00000020,
    # 媒体移除
    SHCNE_MEDIAREMOVED = 0x00000040,
    # 驱动器移除
    SHCNE_DRIVEREMOVED = 0x00000080,
    # 驱动器添加
    SHCNE_DRIVEADD = 0x00000100,
    # 网络共享
    SHCNE_NETSHARE = 0x00000200,
    # 网络取消共享
    SHCNE_NETUNSHARE = 0x00000400,
    # 属性更改
    SHCNE_ATTRIBUTES = 0x00000800,
    # 更新目录
    SHCNE_UPDATEDIR = 0x00001000,
    # 更新项
    SHCNE_UPDATEITEM = 0x00002000,
    # 服务器断开连接
    SHCNE_SERVERDISCONNECT = 0x00004000,
    # 更新图像
    SHCNE_UPDATEIMAGE = 0x00008000,
    # 驱动器添加 GUI
    SHCNE_DRIVEADDGUI = 0x00010000,
    # 重命名文件夹
    SHCNE_RENAMEFOLDER = 0x00020000,
    # 空闲空间更改
    SHCNE_FREESPACE = 0x00040000,
    # 扩展事件
    SHCNE_EXTENDED_EVENT = 0x04000000,
    # 关联更改
    SHCNE_ASSOCCHANGED = 0x08000000,
    # 磁盘事件
    SHCNE_DISKEVENTS = 0x0002381F,
    # 全局事件
    SHCNE_GLOBALEVENTS = 0x0C0581E0,
    # 所有事件
    SHCNE_ALLEVENTS = 0x7FFFFFFF,
    # 中断事件
    SHCNE_INTERRUPT = 0x80000000,
}

# 定义了系统通知消息的枚举
[Flags]
public enum SHCNF : uint
{
    # ID 列表
    SHCNF_IDLIST = 0x0000,
    # 路径 ANSI
    SHCNF_PATHA = 0x0001,
    # 打印机路径 ANSI
    SHCNF_PRINTERA = 0x0002,
    # DWORD 类型
    SHCNF_DWORD = 0x0003,
    # 路径 Unicode
    SHCNF_PATHW = 0x0005,
    # 打印机路径 Unicode
    SHCNF_PRINTERW = 0x0006,
    # 类型
    SHCNF_TYPE = 0x00FF,
    # 刷新
    SHCNF_FLUSH = 0x1000,
    # 刷新但不等待
    SHCNF_FLUSHNOWAIT = 0x3000,
    # 通知递归
    SHCNF_NOTIFYRECURSIVE = 0x10000,
}

# 定义了主语言 ID 的枚举
public enum PrimaryLanguageID : ushort
{
    # 中立语言
    LANG_NEUTRAL = 0x00,
    # 不变语言
    LANG_INVARIANT = 0x7f,
    # 南非荷兰语
    LANG_AFRIKAANS = 0x36,
    # 阿尔巴尼亚语
    LANG_ALBANIAN = 0x1c,
}
    # 阿尔萨斯语语言代码
    LANG_ALSATIAN = 0x84,
    # 阿姆哈拉语语言代码
    LANG_AMHARIC = 0x5e,
    # 阿拉伯语语言代码
    LANG_ARABIC = 0x01,
    # 亚美尼亚语语言代码
    LANG_ARMENIAN = 0x2b,
    # 阿萨姆语语言代码
    LANG_ASSAMESE = 0x4d,
    # 阿塞拜疆语语言代码
    LANG_AZERI = 0x2c,
    # 阿塞拜疆语（另一种表示）语言代码
    LANG_AZERBAIJANI = 0x2c,
    # 孟加拉语语言代码
    LANG_BANGLA = 0x45,
    # 巴什基尔语语言代码
    LANG_BASHKIR = 0x6d,
    # 巴斯克语语言代码
    LANG_BASQUE = 0x2d,
    # 白俄罗斯语语言代码
    LANG_BELARUSIAN = 0x23,
    # 孟加拉语（另一种表示）语言代码
    LANG_BENGALI = 0x45,
    # 布列塔尼语语言代码
    LANG_BRETON = 0x7e,
    # 波斯尼亚语语言代码
    LANG_BOSNIAN = 0x1a,
    # 波斯尼亚语（中立）语言代码
    LANG_BOSNIAN_NEUTRAL = 0x781a,
    # 保加利亚语语言代码
    LANG_BULGARIAN = 0x02,
    # 加泰罗尼亚语语言代码
    LANG_CATALAN = 0x03,
    # 切罗基语语言代码
    LANG_CHEROKEE = 0x5c,
    # 中文语言代码
    LANG_CHINESE = 0x04,
    # 简体中文语言代码
    LANG_CHINESE_SIMPLIFIED = 0x04,
    # 繁体中文语言代码
    LANG_CHINESE_TRADITIONAL = 0x7c04,
    # 科西嘉语语言代码
    LANG_CORSICAN = 0x83,
    # 克罗地亚语语言代码
    LANG_CROATIAN = 0x1a,
    # 捷克语语言代码
    LANG_CZECH = 0x05,
    # 丹麦语语言代码
    LANG_DANISH = 0x06,
    # 达里语语言代码
    LANG_DARI = 0x8c,
    # 迪维希语语言代码
    LANG_DIVEHI = 0x65,
    # 荷兰语语言代码
    LANG_DUTCH = 0x13,
    # 英语语言代码
    LANG_ENGLISH = 0x09,
    # 爱沙尼亚语语言代码
    LANG_ESTONIAN = 0x25,
    # 法罗语语言代码
    LANG_FAEROESE = 0x38,
    # 菲利比诺语语言代码
    LANG_FILIPINO = 0x64,
    # 芬兰语语言代码
    LANG_FINNISH = 0x0b,
    # 法语语言代码
    LANG_FRENCH = 0x0c,
    # 弗里西语语言代码
    LANG_FRISIAN = 0x62,
    # 富拉语语言代码
    LANG_FULAH = 0x67,
    # 加利西亚语语言代码
    LANG_GALICIAN = 0x56,
    # 格鲁吉亚语语言代码
    LANG_GEORGIAN = 0x37,
    # 德语语言代码
    LANG_GERMAN = 0x07,
    # 希腊语语言代码
    LANG_GREEK = 0x08,
    # 格林兰语语言代码
    LANG_GREENLANDIC = 0x6f,
    # 古吉拉特语语言代码
    LANG_GUJARATI = 0x47,
    # 豪萨语语言代码
    LANG_HAUSA = 0x68,
    # 夏威夷语语言代码
    LANG_HAWAIIAN = 0x75,
    # 希伯来语语言代码
    LANG_HEBREW = 0x0d,
    # 印地语语言代码
    LANG_HINDI = 0x39,
    # 匈牙利语语言代码
    LANG_HUNGARIAN = 0x0e,
    # 冰岛语语言代码
    LANG_ICELANDIC = 0x0f,
    # 伊博语语言代码
    LANG_IGBO = 0x70,
    # 印度尼西亚语语言代码
    LANG_INDONESIAN = 0x21,
    # 伊努克提图特语语言代码
    LANG_INUKTITUT = 0x5d,
    # 爱尔兰语语言代码
    LANG_IRISH = 0x3c,
    # 意大利语语言代码
    LANG_ITALIAN = 0x10,
    # 日语语言代码
    LANG_JAPANESE = 0x11,
    # 卡纳达语语言代码
    LANG_KANNADA = 0x4b,
    # 克什米尔语语言代码
    LANG_KASHMIRI = 0x60,
    # 哈萨克语语言代码
    LANG_KAZAK = 0x3f,
    # 高棉语语言代码
    LANG_KHMER = 0x53,
    # 基切语语言代码
    LANG_KICHE = 0x86,
    # 基尼亚尔万达语语言代码
    LANG_KINYARWANDA = 0x87,
    # 孔卡尼语语言代码
    LANG_KONKANI = 0x57,
    # 韩语语言代码
    LANG_KOREAN = 0x12,
    # 吉尔吉斯语语言代码
    LANG_KYRGYZ = 0x40,
    # 老挝语语言代码
    LANG_LAO = 0x54,
    # 拉脱维亚语语言代码
    LANG_LATVIAN = 0x26,
    # 立陶宛语语言代码
    LANG_LITHUANIAN = 0x27,
    # 下索布语语言代码
    LANG_LOWER_SORBIAN = 0x2e,
    # 卢森堡语语言代码
    LANG_LUXEMBOURGISH = 0x6e,
    # 马来语语言代码
    LANG_MALAY = 0x3e,
    # 马拉雅拉姆语语言代码
    LANG_MALAYALAM = 0x4c,
    # 马耳他语语言代码
    LANG_MALTESE = 0x3a,
    # 曼尼普尔语语言代码
    LANG_MANIPURI = 0x58,
    # 毛利语语言代码
    LANG_MAORI = 0x81,
    # 马普切语语言代码
    LANG_MAPUDUNGUN = 0x7a,
    # 马拉地语语言代码
    LANG_MARATHI = 0x4e,
    # 莫霍克语语言代码
    LANG_MOHAWK = 0x7c,
    # 蒙古语语言代码
    LANG_MONGOLIAN = 0x50,
    # 尼泊尔语语言代码
    LANG_NEPALI = 0x61,
    # 挪威语语言代码
    LANG_NORWEGIAN = 0x14,
    # 奥克语语言代码
    LANG_OCCITAN = 0x82,
    # 奥里亚语语言代码
    LANG_ODIA = 0x48,
    # 奥里亚语（另一种表示）语言代码
    LANG_ORIYA = 0x48,
    # 普什图语语言代码
    LANG_PASHTO = 0x63,
    # 波斯语语言代码
    LANG_PERSIAN = 0x29,
    # 波兰语语言代码
    LANG_POLISH = 0x15,
    # 葡萄牙语语言代码
    LANG_PORTUGUESE = 0x16,
    # 普拉语语言代码
    LANG_PULAR = 0x67,
    # 旁遮普语语言代码
    LANG_PUNJABI = 0x46,
    # 凯楚亚语语言代码
    LANG_QUECHUA = 0x6b,
    # 罗马尼亚语语言代码
    LANG_ROMANIAN = 0x18,
    # 罗曼什语语言代码
    LANG_ROMANSH = 0x17,
    # 俄语语言代码
    LANG_RUSSIAN = 0x19,
    # 萨哈语语言代码
    LANG_SAKHA = 0x85,
    # 萨米语语言代码
    LANG_SAMI = 0x3b,
    # 梵语语言代码
    LANG_SANSKRIT = 0x4f,
    # 苏格兰盖尔语语言代码
    LANG_SCOTTISH_GAELIC = 0x91,
    # 定义常量 LANG_TURKISH，值为 0x1f
        LANG_TURKISH = 0x1f,
        # 定义常量 LANG_TURKMEN，值为 0x42
        LANG_TURKMEN = 0x42,
        # 定义常量 LANG_UIGHUR，值为 0x80
        LANG_UIGHUR = 0x80,
        # 定义常量 LANG_UKRAINIAN，值为 0x22
        LANG_UKRAINIAN = 0x22,
        # 定义常量 LANG_UPPER_SORBIAN，值为 0x2e
        LANG_UPPER_SORBIAN = 0x2e,
        # 定义常量 LANG_URDU，值为 0x20
        LANG_URDU = 0x20,
        # 定义常量 LANG_UZBEK，值为 0x43
        LANG_UZBEK = 0x43,
        # 定义常量 LANG_VALENCIAN，值为 0x03
        LANG_VALENCIAN = 0x03,
        # 定义常量 LANG_VIETNAMESE，值为 0x2a
        LANG_VIETNAMESE = 0x2a,
        # 定义常量 LANG_WELSH，值为 0x52
        LANG_WELSH = 0x52,
        # 定义常量 LANG_WOLOF，值为 0x88
        LANG_WOLOF = 0x88,
        # 定义常量 LANG_XHOSA，值为 0x34
        LANG_XHOSA = 0x34,
        # 定义常量 LANG_YAKUT，值为 0x85
        LANG_YAKUT = 0x85,
        # 定义常量 LANG_YI，值为 0x78
        LANG_YI = 0x78,
        # 定义常量 LANG_YORUBA，值为 0x6a
        LANG_YORUBA = 0x6a,
        # 定义常量 LANG_ZULU，值为 0x35
        LANG_ZULU = 0x35,
}
# 定义一个枚举类型 SublanguageID，基于 ushort 类型（无符号 16 位整数）
public enum SublanguageID : ushort
{
    # 默认子语言标识
    SUBLANG_DEFAULT = 0x01,
    # 系统默认子语言标识
    SUBLANG_SYS_DEFAULT = 0x02,
    # 自定义默认子语言标识
    SUBLANG_CUSTOM_DEFAULT = 0x03,
    # 自定义未指定子语言标识
    SUBLANG_CUSTOM_UNSPECIFIED = 0x04,
    # 自定义 UI 默认子语言标识
    SUBLANG_UI_CUSTOM_DEFAULT = 0x05,
    # 南非荷兰语子语言标识
    SUBLANG_AFRIKAANS_SOUTH_AFRICA = 0x01,
    # 阿尔巴尼亚语子语言标识
    SUBLANG_ALBANIAN_ALBANIA = 0x01,
    # 阿尔萨斯语子语言标识
    SUBLANG_ALSATIAN_FRANCE = 0x01,
    # 阿姆哈拉语子语言标识
    SUBLANG_AMHARIC_ETHIOPIA = 0x01,
    # 阿拉伯语（沙特阿拉伯）子语言标识
    SUBLANG_ARABIC_SAUDI_ARABIA = 0x01,
    # 阿拉伯语（伊拉克）子语言标识
    SUBLANG_ARABIC_IRAQ = 0x02,
    # 阿拉伯语（埃及）子语言标识
    SUBLANG_ARABIC_EGYPT = 0x03,
    # 阿拉伯语（利比亚）子语言标识
    SUBLANG_ARABIC_LIBYA = 0x04,
    # 阿拉伯语（阿尔及利亚）子语言标识
    SUBLANG_ARABIC_ALGERIA = 0x05,
    # 阿拉伯语（摩洛哥）子语言标识
    SUBLANG_ARABIC_MOROCCO = 0x06,
    # 阿拉伯语（突尼斯）子语言标识
    SUBLANG_ARABIC_TUNISIA = 0x07,
    # 阿拉伯语（阿曼）子语言标识
    SUBLANG_ARABIC_OMAN = 0x08,
    # 阿拉伯语（也门）子语言标识
    SUBLANG_ARABIC_YEMEN = 0x09,
    # 阿拉伯语（叙利亚）子语言标识
    SUBLANG_ARABIC_SYRIA = 0x0a,
    # 阿拉伯语（约旦）子语言标识
    SUBLANG_ARABIC_JORDAN = 0x0b,
    # 阿拉伯语（黎巴嫩）子语言标识
    SUBLANG_ARABIC_LEBANON = 0x0c,
    # 阿拉伯语（科威特）子语言标识
    SUBLANG_ARABIC_KUWAIT = 0x0d,
    # 阿拉伯语（阿联酋）子语言标识
    SUBLANG_ARABIC_UAE = 0x0e,
    # 阿拉伯语（巴林）子语言标识
    SUBLANG_ARABIC_BAHRAIN = 0x0f,
    # 阿拉伯语（卡塔尔）子语言标识
    SUBLANG_ARABIC_QATAR = 0x10,
    # 亚美尼亚语子语言标识
    SUBLANG_ARMENIAN_ARMENIA = 0x01,
    # 阿萨姆语子语言标识
    SUBLANG_ASSAMESE_INDIA = 0x01,
    # 拉丁阿泽里语子语言标识
    SUBLANG_AZERI_LATIN = 0x01,
    # 西里尔阿泽里语子语言标识
    SUBLANG_AZERI_CYRILLIC = 0x02,
    # 拉丁阿塞拜疆语子语言标识
    SUBLANG_AZERBAIJANI_AZERBAIJAN_LATIN = 0x01,
    # 西里尔阿塞拜疆语子语言标识
    SUBLANG_AZERBAIJANI_AZERBAIJAN_CYRILLIC = 0x02,
    # 印度孟加拉语子语言标识
    SUBLANG_BANGLA_INDIA = 0x01,
    # 孟加拉国孟加拉语子语言标识
    SUBLANG_BANGLA_BANGLADESH = 0x02,
    # 巴什基尔语（俄罗斯）子语言标识
    SUBLANG_BASHKIR_RUSSIA = 0x01,
    # 巴斯克语子语言标识
    SUBLANG_BASQUE_BASQUE = 0x01,
    # 白俄罗斯语子语言标识
    SUBLANG_BELARUSIAN_BELARUS = 0x01,
    # 印度孟加拉语子语言标识
    SUBLANG_BENGALI_INDIA = 0x01,
    # 孟加拉国孟加拉语子语言标识
    SUBLANG_BENGALI_BANGLADESH = 0x02,
    # 波斯尼亚语（拉丁）子语言标识
    SUBLANG_BOSNIAN_BOSNIA_HERZEGOVINA_LATIN = 0x05,
    # 波斯尼亚语（西里尔）子语言标识
    SUBLANG_BOSNIAN_BOSNIA_HERZEGOVINA_CYRILLIC = 0x08,
    # 布列塔尼语子语言标识
    SUBLANG_BRETON_FRANCE = 0x01,
    # 保加利亚语子语言标识
    SUBLANG_BULGARIAN_BULGARIA = 0x01,
    # 加泰罗尼亚语子语言标识
    SUBLANG_CATALAN_CATALAN = 0x01,
    # 切罗基语子语言标识
    SUBLANG_CHEROKEE_CHEROKEE = 0x01,
    # 繁体中文子语言标识
    SUBLANG_CHINESE_TRADITIONAL = 0x01,
    # 简体中文子语言标识
    SUBLANG_CHINESE_SIMPLIFIED = 0x02,
    # 香港中文子语言标识
    SUBLANG_CHINESE_HONGKONG = 0x03,
    # 新加坡中文子语言标识
    SUBLANG_CHINESE_SINGAPORE = 0x04,
    # 澳门中文子语言标识
    SUBLANG_CHINESE_MACAU = 0x05,
    # 科西嘉语子语言标识
    SUBLANG_CORSICAN_FRANCE = 0x01,
    # 捷克语子语言标识
    SUBLANG_CZECH_CZECH_REPUBLIC = 0x01,
    # 克罗地亚语子语言标识
    SUBLANG_CROATIAN_CROATIA = 0x01,
    # 克罗地亚语（波斯尼亚和黑塞哥维那拉丁）子语言标识
    SUBLANG_CROATIAN_BOSNIA_HERZEGOVINA_LATIN = 0x04,
    # 丹麦语子语言标识
    SUBLANG_DANISH_DENMARK = 0x01,
    # 达里语子语言标识
    SUBLANG_DARI_AFGHANISTAN = 0x01,
    # 迪维希语子语言标识
    SUBLANG_DIVEHI_MALDIVES = 0x01,
    # 荷兰语子语言标识
    SUBLANG_DUTCH = 0x01,
    # 荷兰语（比利时）子语言标识
    SUBLANG_DUTCH_BELGIAN = 0x02,
    # 英语（美国）子语言标识
    SUBLANG_ENGLISH_US = 0x01,
    # 英语（英国）子语言标识
    SUBLANG_ENGLISH_UK = 0x02,
    # 英语（澳大利亚）子语言标识
    SUBLANG_ENGLISH_AUS = 0x03,
    # 英语（加拿大）子语言标识
    SUBLANG_ENGLISH_CAN = 0x04,
    # 英语（新西兰）子语言标识
    SUBLANG_ENGLISH_NZ = 0x05,
    # 英语（爱尔兰）子语言标识
    SUBLANG_ENGLISH_EIRE = 0x06,
    # 英语（南非）子语言标识
    SUBLANG_ENGLISH_SOUTH_AFRICA = 0x07,
    # 英语（牙买加）子语言标识
    SUBLANG_ENGLISH_JAMAICA = 0x08,
    # 英语（加勒比海地区）子语言标识
    SUBLANG_ENGLISH_CARIBBEAN = 0x09,
    # 英语（伯利兹）子语言标识
    SUBLANG_ENGLISH_BELIZE = 0x0a,
    # 英语（特立尼达）子语言标识
    SUBLANG_ENGLISH_TRINIDAD = 0x0b,
    # 英语（津巴布韦）子
    # 法语（瑞士）语言标识符
        SUBLANG_FRENCH_SWISS = 0x04,
        # 法语（卢森堡）语言标识符
        SUBLANG_FRENCH_LUXEMBOURG = 0x05,
        # 法语（摩纳哥）语言标识符
        SUBLANG_FRENCH_MONACO = 0x06,
        # 弗里斯兰语（荷兰）语言标识符
        SUBLANG_FRISIAN_NETHERLANDS = 0x01,
        # 富拉语（塞内加尔）语言标识符
        SUBLANG_FULAH_SENEGAL = 0x02,
        # 加利西亚语（加利西亚）语言标识符
        SUBLANG_GALICIAN_GALICIAN = 0x01,
        # 格鲁吉亚语（格鲁吉亚）语言标识符
        SUBLANG_GEORGIAN_GEORGIA = 0x01,
        # 德语（默认）语言标识符
        SUBLANG_GERMAN = 0x01,
        # 德语（瑞士）语言标识符
        SUBLANG_GERMAN_SWISS = 0x02,
        # 德语（奥地利）语言标识符
        SUBLANG_GERMAN_AUSTRIAN = 0x03,
        # 德语（卢森堡）语言标识符
        SUBLANG_GERMAN_LUXEMBOURG = 0x04,
        # 德语（列支敦士登）语言标识符
        SUBLANG_GERMAN_LIECHTENSTEIN = 0x05,
        # 希腊语（希腊）语言标识符
        SUBLANG_GREEK_GREECE = 0x01,
        # 格林兰语（格林兰）语言标识符
        SUBLANG_GREENLANDIC_GREENLAND = 0x01,
        # 古吉拉特语（印度）语言标识符
        SUBLANG_GUJARATI_INDIA = 0x01,
        # 豪萨语（尼日利亚-拉丁）语言标识符
        SUBLANG_HAUSA_NIGERIA_LATIN = 0x01,
        # 夏威夷语（美国）语言标识符
        SUBLANG_HAWAIIAN_US = 0x01,
        # 希伯来语（以色列）语言标识符
        SUBLANG_HEBREW_ISRAEL = 0x01,
        # 印地语（印度）语言标识符
        SUBLANG_HINDI_INDIA = 0x01,
        # 匈牙利语（匈牙利）语言标识符
        SUBLANG_HUNGARIAN_HUNGARY = 0x01,
        # 冰岛语（冰岛）语言标识符
        SUBLANG_ICELANDIC_ICELAND = 0x01,
        # 伊博语（尼日利亚）语言标识符
        SUBLANG_IGBO_NIGERIA = 0x01,
        # 印尼语（印度尼西亚）语言标识符
        SUBLANG_INDONESIAN_INDONESIA = 0x01,
        # 因纽特语（加拿大）语言标识符
        SUBLANG_INUKTITUT_CANADA = 0x01,
        # 因纽特语（加拿大-拉丁）语言标识符
        SUBLANG_INUKTITUT_CANADA_LATIN = 0x02,
        # 爱尔兰语（爱尔兰）语言标识符
        SUBLANG_IRISH_IRELAND = 0x02,
        # 意大利语（默认）语言标识符
        SUBLANG_ITALIAN = 0x01,
        # 意大利语（瑞士）语言标识符
        SUBLANG_ITALIAN_SWISS = 0x02,
        # 日语（日本）语言标识符
        SUBLANG_JAPANESE_JAPAN = 0x01,
        # 卡纳达语（印度）语言标识符
        SUBLANG_KANNADA_INDIA = 0x01,
        # 哈萨克语（哈萨克斯坦）语言标识符
        SUBLANG_KAZAK_KAZAKHSTAN = 0x01,
        # 高棉语（柬埔寨）语言标识符
        SUBLANG_KHMER_CAMBODIA = 0x01,
        # 基切语（危地马拉）语言标识符
        SUBLANG_KICHE_GUATEMALA = 0x01,
        # 基尼亚尔万达语（卢旺达）语言标识符
        SUBLANG_KINYARWANDA_RWANDA = 0x01,
        # 孔卡尼语（印度）语言标识符
        SUBLANG_KONKANI_INDIA = 0x01,
        # 韩语（默认）语言标识符
        SUBLANG_KOREAN = 0x01,
        # 吉尔吉斯语（吉尔吉斯斯坦）语言标识符
        SUBLANG_KYRGYZ_KYRGYZSTAN = 0x01,
        # 老挝语（老挝）语言标识符
        SUBLANG_LAO_LAO = 0x01,
        # 拉脱维亚语（拉脱维亚）语言标识符
        SUBLANG_LATVIAN_LATVIA = 0x01,
        # 立陶宛语（默认）语言标识符
        SUBLANG_LITHUANIAN = 0x01,
        # 下索布语（德国）语言标识符
        SUBLANG_LOWER_SORBIAN_GERMANY = 0x02,
        # 卢森堡语（卢森堡）语言标识符
        SUBLANG_LUXEMBOURGISH_LUXEMBOURG = 0x01,
        # 马来语（马来西亚）语言标识符
        SUBLANG_MALAY_MALAYSIA = 0x01,
        # 马来语（文莱达鲁萨兰）语言标识符
        SUBLANG_MALAY_BRUNEI_DARUSSALAM = 0x02,
        # 马拉雅拉姆语（印度）语言标识符
        SUBLANG_MALAYALAM_INDIA = 0x01,
        # 马耳他语（马耳他）语言标识符
        SUBLANG_MALTESE_MALTA = 0x01,
        # 毛利语（新西兰）语言标识符
        SUBLANG_MAORI_NEW_ZEALAND = 0x01,
        # 马普杜根语（智利）语言标识符
        SUBLANG_MAPUDUNGUN_CHILE = 0x01,
        # 马拉地语（印度）语言标识符
        SUBLANG_MARATHI_INDIA = 0x01,
        # 莫霍克语（莫霍克）语言标识符
        SUBLANG_MOHAWK_MOHAWK = 0x01,
        # 蒙古语（西里尔字母，蒙古国）语言标识符
        SUBLANG_MONGOLIAN_CYRILLIC_MONGOLIA = 0x01,
        # 蒙古语（简体字，中华人民共和国）语言标识符
        SUBLANG_MONGOLIAN_PRC = 0x02,
        # 尼泊尔语（印度）语言标识符
        SUBLANG_NEPALI_INDIA = 0x02,
        # 尼泊尔语（尼泊尔）语言标识符
        SUBLANG_NEPALI_NEPAL = 0x01,
        # 挪威语（书面语）语言标识符
        SUBLANG_NORWEGIAN_BOKMAL = 0x01,
        # 挪威语（新挪威语）语言标识符
        SUBLANG_NORWEGIAN_NYNORSK = 0x02,
        # 奥克语（法国）语言标识符
        SUBLANG_OCCITAN_FRANCE = 0x01,
        # 奥里亚语（印度）语言标识符
        SUBLANG_ODIA_INDIA = 0x01,
        # 奥里亚语（印度）语言标识符
        SUBLANG_ORIYA_INDIA = 0x01,
        # 普什图语（阿富汗）语言标识符
        SUBLANG_PASHTO_AFGHANISTAN = 0x01,
        # 波斯语（伊朗）语言标识符
        SUBLANG_PERSIAN_IRAN = 0x01,
        # 波兰语（波兰）语言标识符
        SUBLANG_POLISH_POLAND = 0x01,
        # 葡萄牙语（默认）语言标识符
        SUBLANG_PORTUGUESE = 0x02,
        # 葡萄牙语（巴西）语言标识符
        SUBLANG_PORTUGUESE_BRAZILIAN = 0x01,
        # 普拉尔语（塞内加尔）语言标识符
        SUBLANG_PULAR_SENEGAL = 0x02,
        # 旁遮普语（印度）语言标识符
        SUBLANG
    # 定义 SAMI (Subtitles for the Deaf) 的 Inari-Finland 方言
    SUBLANG_SAMI_INARI_FINLAND = 0x09,
    # 定义印度的梵语
    SUBLANG_SANSKRIT_INDIA = 0x01,
    # 定义苏格兰盖尔语
    SUBLANG_SCOTTISH_GAELIC = 0x01,
    # 定义波斯尼亚和黑塞哥维那的拉丁语塞尔维亚语
    SUBLANG_SERBIAN_BOSNIA_HERZEGOVINA_LATIN = 0x06,
    # 定义波斯尼亚和黑塞哥维那的西里尔字母塞尔维亚语
    SUBLANG_SERBIAN_BOSNIA_HERZEGOVINA_CYRILLIC = 0x07,
    # 定义黑山的拉丁语塞尔维亚语
    SUBLANG_SERBIAN_MONTENEGRO_LATIN = 0x0b,
    # 定义黑山的西里尔字母塞尔维亚语
    SUBLANG_SERBIAN_MONTENEGRO_CYRILLIC = 0x0c,
    # 定义塞尔维亚的拉丁语
    SUBLANG_SERBIAN_SERBIA_LATIN = 0x09,
    # 定义塞尔维亚的西里尔字母
    SUBLANG_SERBIAN_SERBIA_CYRILLIC = 0x0a,
    # 定义克罗地亚的塞尔维亚语
    SUBLANG_SERBIAN_CROATIA = 0x01,
    # 定义塞尔维亚语的拉丁字母
    SUBLANG_SERBIAN_LATIN = 0x02,
    # 定义塞尔维亚语的西里尔字母
    SUBLANG_SERBIAN_CYRILLIC = 0x03,
    # 定义印度的信德语
    SUBLANG_SINDHI_INDIA = 0x01,
    # 定义巴基斯坦的信德语
    SUBLANG_SINDHI_PAKISTAN = 0x02,
    # 定义阿富汗的信德语
    SUBLANG_SINDHI_AFGHANISTAN = 0x02,
    # 定义斯里兰卡的僧伽罗语
    SUBLANG_SINHALESE_SRI_LANKA = 0x01,
    # 定义南非北部的索托语
    SUBLANG_SOTHO_NORTHERN_SOUTH_AFRICA = 0x01,
    # 定义斯洛伐克的斯洛伐克语
    SUBLANG_SLOVAK_SLOVAKIA = 0x01,
    # 定义斯洛文尼亚的斯洛文尼亚语
    SUBLANG_SLOVENIAN_SLOVENIA = 0x01,
    # 定义西班牙语
    SUBLANG_SPANISH = 0x01,
    # 定义墨西哥的西班牙语
    SUBLANG_SPANISH_MEXICAN = 0x02,
    # 定义现代西班牙语
    SUBLANG_SPANISH_MODERN = 0x03,
    # 定义危地马拉的西班牙语
    SUBLANG_SPANISH_GUATEMALA = 0x04,
    # 定义哥斯达黎加的西班牙语
    SUBLANG_SPANISH_COSTA_RICA = 0x05,
    # 定义巴拿马的西班牙语
    SUBLANG_SPANISH_PANAMA = 0x06,
    # 定义多米尼加共和国的西班牙语
    SUBLANG_SPANISH_DOMINICAN_REPUBLIC = 0x07,
    # 定义委内瑞拉的西班牙语
    SUBLANG_SPANISH_VENEZUELA = 0x08,
    # 定义哥伦比亚的西班牙语
    SUBLANG_SPANISH_COLOMBIA = 0x09,
    # 定义秘鲁的西班牙语
    SUBLANG_SPANISH_PERU = 0x0a,
    # 定义阿根廷的西班牙语
    SUBLANG_SPANISH_ARGENTINA = 0x0b,
    # 定义厄瓜多尔的西班牙语
    SUBLANG_SPANISH_ECUADOR = 0x0c,
    # 定义智利的西班牙语
    SUBLANG_SPANISH_CHILE = 0x0d,
    # 定义乌拉圭的西班牙语
    SUBLANG_SPANISH_URUGUAY = 0x0e,
    # 定义巴拉圭的西班牙语
    SUBLANG_SPANISH_PARAGUAY = 0x0f,
    # 定义玻利维亚的西班牙语
    SUBLANG_SPANISH_BOLIVIA = 0x10,
    # 定义萨尔瓦多的西班牙语
    SUBLANG_SPANISH_EL_SALVADOR = 0x11,
    # 定义洪都拉斯的西班牙语
    SUBLANG_SPANISH_HONDURAS = 0x12,
    # 定义尼加拉瓜的西班牙语
    SUBLANG_SPANISH_NICARAGUA = 0x13,
    # 定义波多黎各的西班牙语
    SUBLANG_SPANISH_PUERTO_RICO = 0x14,
    # 定义美国的西班牙语
    SUBLANG_SPANISH_US = 0x15,
    # 定义肯尼亚的斯瓦希里语
    SUBLANG_SWAHILI_KENYA = 0x01,
    # 定义瑞典语
    SUBLANG_SWEDISH = 0x01,
    # 定义芬兰的瑞典语
    SUBLANG_SWEDISH_FINLAND = 0x02,
    # 定义叙利亚的叙利亚语
    SUBLANG_SYRIAC_SYRIA = 0x01,
    # 定义塔吉克斯坦的塔吉克语
    SUBLANG_TAJIK_TAJIKISTAN = 0x01,
    # 定义阿尔及利亚的塔马泽特语（拉丁字母）
    SUBLANG_TAMAZIGHT_ALGERIA_LATIN = 0x02,
    # 定义摩洛哥的塔马泽特语（提非纳语）
    SUBLANG_TAMAZIGHT_MOROCCO_TIFINAGH = 0x04,
    # 定义印度的泰米尔语
    SUBLANG_TAMIL_INDIA = 0x01,
    # 定义斯里兰卡的泰米尔语
    SUBLANG_TAMIL_SRI_LANKA = 0x02,
    # 定义俄罗斯的鞑靼语
    SUBLANG_TATAR_RUSSIA = 0x01,
    # 定义印度的泰卢固语
    SUBLANG_TELUGU_INDIA = 0x01,
    # 定义泰国的泰语
    SUBLANG_THAI_THAILAND = 0x01,
    # 定义中国大陆的藏语
    SUBLANG_TIBETAN_PRC = 0x01,
    # 定义厄立特里亚的提格雷语
    SUBLANG_TIGRIGNA_ERITREA = 0x02,
    # 定义厄立特里亚的提格里尼亚语
    SUBLANG_TIGRINYA_ERITREA = 0x02,
    # 定义埃塞俄比亚的提格里尼亚语
    SUBLANG_TIGRINYA_ETHIOPIA = 0x01,
    # 定义博茨瓦纳的茨瓦纳语
    SUBLANG_TSWANA_BOTSWANA = 0x02,
    # 定义南非的茨瓦纳语
    SUBLANG_TSWANA_SOUTH_AFRICA = 0x01,
    # 定义土耳其的土耳其语
    SUBLANG_TURKISH_TURKEY = 0x01,
    # 定义土库曼斯坦的土库曼语
    SUBLANG_TURKMEN_TURKMENISTAN = 0x01,
    # 定义中国大陆的维吾尔语
    SUBLANG_UIGHUR_PRC = 0x01,
    # 定义乌克兰的乌克兰语
    SUBLANG_UKRAINIAN_UKRAINE = 0x01,
    # 定义德国的上索布语
    SUBLANG_UPPER_SORBIAN_GERMANY = 0x01,
    # 定义巴基斯坦的乌尔都语
    SUBLANG_URDU_PAKISTAN = 0x01,
    # 定义印度的乌尔都语
    SUBLANG_URDU_INDIA = 0x02,
    # 定义拉丁字母的乌兹别克语
    SUBLANG_UZBEK_LATIN = 0x01,
    # 定义西里尔字母的乌兹别克语
}
# 结束前一个代码块（例如类或方法）

# 定义一个枚举，表示窗口的背景效果类型
public enum WindowBackdropType
{
    # 没有背景效果
    None,

    # 设置 <c>DWMWA_SYSTEMBACKDROP_TYPE</c> 为 <see langword="0"></see>
    Auto,

    # Windows 11 Mica 效果
    Mica,

    # Windows Acrylic 效果
    Acrylic,

    # Windows 11 壁纸模糊效果
    Tabbed,
}

# 定义一个枚举，表示应用程序使用 WPF UI 时的主题
public enum ApplicationTheme
{
    # 未知应用程序主题
    Unknown,

    # 深色应用程序主题
    Dark,

    # 浅色应用程序主题
    Light,

    # 高对比度应用程序主题
    HighContrast,
}
```