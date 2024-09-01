# `.\better-genshin-impact\BetterGenshinImpact\GameTask\AutoSkip\AutoSkipConfig.cs`

```cs
# 自动跳过剧情配置类，支持属性绑定和观察
[Serializable]
public partial class AutoSkipConfig : ObservableObject
{
    # 触发器是否启用，启用后将自动处理对话跳过和选项点击
    [ObservableProperty] private bool _enabled = true;

    # 启用快速跳过对话
    [ObservableProperty] private bool _quicklySkipConversationsEnabled = true;

    # 聊天选项文本宽度
    public int ChatOptionTextWidth { get; set; } = 280;

    # 探索选项文本宽度
    public int ExpeditionOptionTextWidth { get; set; } = 130;

    # 选择选项前的延迟（毫秒）
    [ObservableProperty] private int _afterChooseOptionSleepDelay = 0;

    # 自动领取每日委托奖励
    [ObservableProperty] private bool _autoGetDailyRewardsEnabled = true;

    # 自动重新派遣
    [ObservableProperty] private bool _autoReExploreEnabled = true;

    # 自动重新派遣使用角色配置（已废弃）
    [Obsolete]
    [ObservableProperty] private string _autoReExploreCharacter = "";

    # 优先选择第一个选项、最后一个选项或不选择
    [ObservableProperty] private string _clickChatOption = "优先选择最后一个选项";

    # 自动邀约启用
    [ObservableProperty] private bool _autoHangoutEventEnabled = false;

    # 自动邀约分支选择
    [ObservableProperty] private string _autoHangoutEndChoose = string.Empty;

    # 自动邀约选择选项前的延迟（毫秒）
    [ObservableProperty] private int _autoHangoutChooseOptionSleepDelay = 0;

    # 自动邀约时自动点击跳过按钮
    [ObservableProperty] private bool _autoHangoutPressSkipEnabled = true;

    # 判断是否选择第一个聊天选项
    public bool IsClickFirstChatOption()
    {
        return ClickChatOption == "优先选择第一个选项";
    }

    # 判断是否随机选择聊天选项
    public bool IsClickRandomChatOption()
    {
        return ClickChatOption == "随机选择选项";
    }

    # 判断是否不选择聊天选项
    public bool IsClickNoneChatOption()
    {
        return ClickChatOption == "不选择选项";
    }

    # 后台运行启用
    [ObservableProperty] private bool _runBackgroundEnabled = false;

    # 提交物品启用
    [ObservableProperty] private bool _submitGoodsEnabled = true;

    # 关闭弹出层启用
    [ObservableProperty] private bool _closePopupPagedEnabled = true;
}
```