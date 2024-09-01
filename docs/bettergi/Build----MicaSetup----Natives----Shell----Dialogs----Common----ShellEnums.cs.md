# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\Common\ShellEnums.cs`

```cs
# 定义显示名称类型的枚举
﻿namespace MicaSetup.Shell.Dialogs;

public enum DisplayNameType
{
    # 默认显示名称
    Default = 0x00000000,
    # 相对父级显示名称
    RelativeToParent = unchecked((int)0x80018001),
    # 相对父级地址栏显示名称
    RelativeToParentAddressBar = unchecked((int)0x8007c001),
    # 相对桌面显示名称
    RelativeToDesktop = unchecked((int)0x80028000),
    # 相对父级编辑显示名称
    RelativeToParentEditing = unchecked((int)0x80031001),
    # 相对桌面编辑显示名称
    RelativeToDesktopEditing = unchecked((int)0x8004c000),
    # 文件系统路径显示名称
    FileSystemPath = unchecked((int)0x80058000),
    # URL 显示名称
    Url = unchecked((int)0x80068000),
}

# 定义文件对话框添加位置的枚举
public enum FileDialogAddPlaceLocation
{
    # 添加到对话框底部
    Bottom = 0x00000000,
    # 添加到对话框顶部
    Top = 0x00000001,
}

# 定义文件夹逻辑视图模式的枚举
public enum FolderLogicalViewMode
{
    # 未指定视图模式
    Unspecified = -1,
    # 无视图模式
    None = 0,
    # 首个视图模式
    First = 1,
    # 详细视图模式
    Details = 1,
    # 瓷砖视图模式
    Tiles = 2,
    # 图标视图模式
    Icons = 3,
    # 列表视图模式
    List = 4,
    # 内容视图模式
    Content = 5,
    # 最后一个视图模式
    Last = 5,
}

# 定义库文件夹类型的枚举
public enum LibraryFolderType
{
    # 通用文件夹
    Generic = 0,
    # 文档文件夹
    Documents,
    # 音乐文件夹
    Music,
    # 图片文件夹
    Pictures,
    # 视频文件夹
    Videos,
}

# 定义查询解析器管理选项的枚举
public enum QueryParserManagerOption
{
    # 架构二进制名称
    SchemaBinaryName = 0,
    # 预本地化架构二进制路径
    PreLocalizedSchemaBinaryPath = 1,
    # 未本地化架构二进制路径
    UnlocalizedSchemaBinaryPath = 2,
    # 本地化架构二进制路径
    LocalizedSchemaBinaryPath = 3,
    # 将 LCID 附加到本地化路径
    AppendLCIDToLocalizedPath = 4,
    # 本地化支持
    LocalizerSupport = 5,
}

# 定义搜索条件操作的枚举
public enum SearchConditionOperation
{
    # 隐式操作
    Implicit = 0,
    # 等于操作
    Equal = 1,
    # 不等于操作
    NotEqual = 2,
    # 小于操作
    LessThan = 3,
    # 大于操作
    GreaterThan = 4,
    # 小于或等于操作
    LessThanOrEqual = 5,
    # 大于或等于操作
    GreaterThanOrEqual = 6,
    # 值以指定内容开头
    ValueStartsWith = 7,
    # 值以指定内容结尾
    ValueEndsWith = 8,
    # 值包含指定内容
    ValueContains = 9,
    # 值不包含指定内容
    ValueNotContains = 10,
    # DOS 通配符
    DosWildcards = 11,
    # 单词等于
    WordEqual = 12,
    # 单词以指定内容开头
    WordStartsWith = 13,
    # 应用程序特定操作
    ApplicationSpecific = 14,
}

# 定义搜索条件类型的枚举
public enum SearchConditionType
{
    # 与操作
    And = 0,
    # 或操作
    Or = 1,
    # 非操作
    Not = 2,
    # 叶子条件
    Leaf = 3,
}

# 定义排序方向的枚举
public enum SortDirection
{
    # 默认排序
    Default = 0,
    # 降序排序
    Descending = -1,
    # 升序排序
    Ascending = 1,
}

# 定义结构化查询多选项的枚举
public enum StructuredQueryMultipleOption
{
    # 虚拟属性
    VirtualProperty,
    # 默认属性
    DefaultProperty,
    # 类型生成器
    GeneratorForType,
    # 映射属性
    MapProperty,
}

# 定义结构化查询单选项的枚举
public enum StructuredQuerySingleOption
{
    # 架构
    Schema,
    # 区域设置
    Locale,
    # 单词断词器
    WordBreaker,
    # 自然语法
    NaturalSyntax,
    # 自动通配符
    AutomaticWildcard,
    # 跟踪级别
    TraceLevel,
    # 语言关键字
    LanguageKeywords,
    # 语法
    Syntax,
    # 时区
    TimeZone,
    # 隐式连接器
    ImplicitConnector,
    # 连接器大小写
    ConnectorCase,
}

# 定义窗口显示命令的枚举
public enum WindowShowCommand
{
    # 隐藏窗口
    Hide = 0,
    # 正常显示窗口
    Normal = 1,
    # 最小化窗口
    Minimized = 2,
    # 最大化窗口
    Maximized = 3,
    # 显示但不激活窗口
    ShowNoActivate = 4,
    # 显示窗口
    Show = 5,
    # 最小化窗口
    Minimize = 6,
    # 显示最小化但不激活窗口
    ShowMinimizedNoActivate = 7,
    # 显示窗口（NA）
    ShowNA = 8,
    # 恢复窗口
    Restore = 9,
    # 默认显示窗口
    Default = 10,
    # 强制最小化窗口
    ForceMinimize = 11,
}
```