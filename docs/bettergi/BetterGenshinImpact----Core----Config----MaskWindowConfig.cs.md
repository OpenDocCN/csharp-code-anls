# `.\better-genshin-impact\BetterGenshinImpact\Core\Config\MaskWindowConfig.cs`

```cs
# 使用 CommunityToolkit.Mvvm.ComponentModel 命名空间
using CommunityToolkit.Mvvm.ComponentModel;
# 使用 OpenCvSharp 命名空间
using OpenCvSharp;
# 使用 System 命名空间
using System;
# 引入 System.Windows.Point 命名空间中的 Point 类型
using Point = System.Windows.Point;

# 定义命名空间 BetterGenshinImpact.Core.Config
namespace BetterGenshinImpact.Core.Config
{
    # <summary>
    #     遮罩窗口配置
    # </summary>
    # 标记类为可序列化
    [Serializable]
    # 定义 MaskWindowConfig 类，继承自 ObservableObject
    public partial class MaskWindowConfig : ObservableObject
    {
        # <summary>
        #     控件是否锁定（拖拽移动等）
        # </summary>
        # 定义 _controlLocked 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _controlLocked = true;

        # <summary>
        #     方位提示是否启用
        # </summary>
        # 定义 _directionsEnabled 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _directionsEnabled;

        # <summary>
        #     是否在遮罩窗口上显示识别结果
        # </summary>
        # 定义 _displayRecognitionResultsOnMask 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _displayRecognitionResultsOnMask = true;

        # <summary>
        #     日志窗口位置与大小
        # </summary>
        # 定义 _logBoxLocation 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private Rect _logBoxLocation;

        # <summary>
        #     是否启用遮罩窗口
        # </summary>
        # 定义 _maskEnabled 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _maskEnabled = true;

        # <summary>
        # 显示遮罩窗口边框
        # </summary>
        # [ObservableProperty] private bool _showMaskBorder = false; # 注释掉的属性，未使用

        # <summary>
        #     显示日志窗口
        # </summary>
        # 定义 _showLogBox 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _showLogBox = true;

        # <summary>
        #     显示状态指示
        # </summary>
        # 定义 _showStatus 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _showStatus = true;

        # <summary>
        #     UID遮盖是否启用
        # </summary>
        # 定义 _uidCoverEnabled 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _uidCoverEnabled;

        # <summary>
        #     1080p下UID遮盖的位置与大小
        # </summary>
        # 定义 UidCoverRect 属性，默认值为指定的矩形区域
        public Rect UidCoverRect { get; set; } = new(1690, 1052, 173, 22);

        # <summary>
        #     1080p下UID遮盖的位置与大小
        # </summary>
        # 定义 UidCoverRightBottomRect 属性，默认值为指定的矩形区域
        public Rect UidCoverRightBottomRect { get; set; } = new(1920 - 1690, 1080 - 1052, 173, 22);

        # 定义东部点的位置
        public Point EastPoint { get; set; } = new(274, 109);
        # 定义南部点的位置
        public Point SouthPoint { get; set; } = new(150, 233);
        # 定义西部点的位置
        public Point WestPoint { get; set; } = new(32, 109);
        # 定义北部点的位置
        public Point NorthPoint { get; set; } = new(150, -9);

        # <summary>
        # 显示FPS
        # </summary>
        # 定义 _showFps 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _showFps = false;

        # <summary>
        # 作为原神子窗体
        # 有些bug没解决
        # </summary>
        # 定义 _useSubform 属性，并标记为 ObservableProperty，以便于数据绑定
        [ObservableProperty]
        private bool _useSubform = false;
    }
}
```