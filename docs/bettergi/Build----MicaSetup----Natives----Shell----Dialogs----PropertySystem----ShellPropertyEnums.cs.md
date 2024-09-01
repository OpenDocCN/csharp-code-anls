# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\PropertySystem\ShellPropertyEnums.cs`

```cs
# 定义一个表示属性枚举类型的枚举
public enum PropEnumType
{
    DiscreteValue = 0,  # 表示离散值
    RangedValue = 1,    # 表示范围值
    DefaultValue = 2,   # 表示默认值
    EndRange = 3,       # 表示范围结束
};

# 定义一个表示属性聚合类型的枚举
public enum PropertyAggregationType
{
    Default = 0,        # 默认聚合类型
    First = 1,          # 取第一个值
    Sum = 2,            # 求和
    Average = 3,        # 求平均值
    DateRange = 4,      # 日期范围
    Union = 5,          # 联合
    Max = 6,            # 最大值
    Min = 7,            # 最小值
}

# 定义一个表示属性列状态选项的枚举，使用位标志
[Flags]
public enum PropertyColumnStateOptions
{
    None = 0x00000000,                      # 无状态
    StringType = 0x00000001,                # 字符串类型
    IntegerType = 0x00000002,               # 整数类型
    DateType = 0x00000003,                  # 日期类型
    TypeMask = 0x0000000f,                  # 类型掩码
    OnByDefault = 0x00000010,               # 默认开启
    Slow = 0x00000020,                      # 速度慢
    Extended = 0x00000040,                  # 扩展状态
    SecondaryUI = 0x00000080,               # 辅助 UI
    Hidden = 0x00000100,                    # 隐藏
    PreferVariantCompare = 0x00000200,     # 偏好变体比较
    PreferFormatForDisplay = 0x00000400,   # 偏好显示格式
    NoSortByFolders = 0x00000800,          # 不按文件夹排序
    ViewOnly = 0x00010000,                  # 仅查看
    BatchRead = 0x00020000,                # 批量读取
    NoGroupBy = 0x00040000,                # 不分组
    FixedWidth = 0x00001000,                # 固定宽度
    NoDpiScale = 0x00002000,               # 无 DPI 缩放
    FixedRatio = 0x00004000,               # 固定比例
    DisplayMask = 0x0000F000,              # 显示掩码
}

# 定义一个表示属性条件操作的枚举
public enum PropertyConditionOperation
{
    Implicit,          # 隐式操作
    Equal,             # 等于
    NotEqual,          # 不等于
    LessThan,          # 小于
    GreaterThan,       # 大于
    LessThanOrEqual,   # 小于或等于
    GreaterThanOrEqual,# 大于或等于
    ValueStartsWith,   # 值以...开始
    ValueEndsWith,     # 值以...结束
    ValueContains,     # 值包含...
    ValueNotContains,  # 值不包含...
    DOSWildCards,      # DOS 通配符
    WordEqual,         # 单词等于
    WordStartsWith,    # 单词以...开始
    ApplicationSpecific,# 应用程序特定
}

# 定义一个表示属性条件类型的枚举
public enum PropertyConditionType
{
    None = 0,         # 无条件
    String = 1,       # 字符串
    Size = 2,         # 大小
    DateTime = 3,     # 日期时间
    Boolean = 4,      # 布尔值
    Number = 5,       # 数字
}

# 定义一个表示属性描述格式选项的枚举，使用位标志
[Flags]
public enum PropertyDescriptionFormatOptions
{
    None = 0,                      # 无格式
    PrefixName = 0x1,              # 前缀名称
    FileName = 0x2,                # 文件名
    AlwaysKB = 0x4,                # 始终以 KB 为单位
    RightToLeft = 0x8,             # 从右到左
    ShortTime = 0x10,              # 短时间格式
    LongTime = 0x20,               # 长时间格式
    HideTime = 64,                 # 隐藏时间
    ShortDate = 0x80,              # 短日期格式
    LongDate = 0x100,              # 长日期格式
    HideDate = 0x200,              # 隐藏日期
    RelativeDate = 0x400,          # 相对日期
    UseEditInvitation = 0x800,     # 使用编辑邀请
    ReadOnly = 0x1000,             # 只读
    NoAutoReadingOrder = 0x2000,   # 无自动阅读顺序
    SmartDateTime = 0x4000,        # 智能日期时间
}

# 定义一个表示属性显示类型的枚举
public enum PropertyDisplayType
{
    String = 0,      # 字符串类型
    Number = 1,      # 数字类型
    Boolean = 2,     # 布尔类型
    DateTime = 3,    # 日期时间类型
    Enumerated = 4,  # 枚举类型
}

# 定义一个表示属性分组范围的枚举
public enum PropertyGroupingRange
{
    Discrete = 0,    # 离散范围
    Alphanumeric = 1,# 字母数字范围
    Size = 2,        # 大小范围
    Dynamic = 3,     # 动态范围
    Date = 4,        # 日期范围
    Percent = 5,     # 百分比范围
    Enumerated = 6,  # 枚举范围
}

# 定义一个表示属性排序描述的枚举
public enum PropertySortDescription
{
    General,                # 一般排序
    AToZ,                   # 从 A 到 Z
    LowestToHighest,        # 从最低到最高
    SmallestToBiggest,      # 从最小到最大
    OldestToNewest,         # 从最旧到最新
}

# 定义一个表示属性存储缓存状态的枚举
public enum PropertyStoreCacheState
{
    Normal = 0,     # 正常状态
    NotInSource = 1, # 不在源中
    Dirty = 2       # 脏状态
}

# 定义一个表示属性类型选项的枚举，使用位标志
[Flags]
public enum PropertyTypeOptions
{
    None = 0x00000000,               # 无选项
    MultipleValues = 0x00000001,     # 多个值
    IsInnate = 0x00000002,           # 内在属性
    IsGroup = 0x00000004,            # 分组属性
    CanGroupBy = 0x00000008,         # 可以分组
    CanStackBy = 0x00000010,         # 可以堆叠
    IsTreeProperty = 0x00000020,     # 树状属性
    IncludeInFullTextQuery = 0x00000040,# 包含在全文查询中
    IsViewable = 0x00000080,         # 可查看
    IsQueryable = 0x00000100,        # 可查询
    CanBePurged = 0x00000200,        # 可清除
    IsSystemProperty = unchecked((int)0x80000000), # 系统属性
    MaskAll = unchecked((int)0x800001FF), # 全部掩码
}

# 定义一个表示属性视图选项的枚举，使用位标志
[Flags]
public enum PropertyViewOptions
{
    None = 0x00000000,      # 无视图选项
    CenterAlign = 0x00000001, # 居中对齐
    RightAlign = 0x00000002,  # 右对齐
    BeginNewGroup = 0x00000004, # 开始新分组
    # 定义常量，表示“填充区域”的标志位
    FillArea = 0x00000008,
    # 定义常量，表示“按降序排序”的标志位
    SortDescending = 0x00000010,
    # 定义常量，表示“仅在存在时显示”的标志位
    ShowOnlyIfPresent = 0x00000020,
    # 定义常量，表示“默认显示”的标志位
    ShowByDefault = 0x00000040,
    # 定义常量，表示“在主列表中显示”的标志位
    ShowInPrimaryList = 0x00000080,
    # 定义常量，表示“在次级列表中显示”的标志位
    ShowInSecondaryList = 0x00000100,
    # 定义常量，表示“隐藏标签”的标志位
    HideLabel = 0x00000200,
    # 定义常量，表示“隐藏”的标志位
    Hidden = 0x00000800,
    # 定义常量，表示“可以换行”的标志位
    CanWrap = 0x00001000,
    # 定义常量，表示“所有标志位都被掩盖”的掩码
    MaskAll = 0x000003ff,
# 结束当前代码块或代码结构
}
```