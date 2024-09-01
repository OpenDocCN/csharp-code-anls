# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\NetFw\INetFwEnums.cs`

```cs
# 定义网络防火墙动作的枚举
public enum NET_FW_ACTION
{
    # 阻止动作
    NET_FW_ACTION_BLOCK,
    # 允许动作
    NET_FW_ACTION_ALLOW,
    # 动作的最大值
    NET_FW_ACTION_MAX,
}

# 定义网络防火墙 IP 协议的枚举
public enum NET_FW_IP_PROTOCOL
{
    # TCP 协议，值为 6
    NET_FW_IP_PROTOCOL_TCP = 6,
    # UDP 协议，值为 17
    NET_FW_IP_PROTOCOL_UDP = 17,
    # 任意协议，值为 256
    NET_FW_IP_PROTOCOL_ANY = 256,
}

# 定义网络防火墙 IP 版本的枚举
public enum NET_FW_IP_VERSION
{
    # IPv4 版本
    NET_FW_IP_VERSION_V4,
    # IPv6 版本
    NET_FW_IP_VERSION_V6,
    # 任意 IP 版本
    NET_FW_IP_VERSION_ANY,
    # IP 版本的最大值
    NET_FW_IP_VERSION_MAX,
}

# 定义网络防火墙修改状态的枚举
public enum NET_FW_MODIFY_STATE
{
    # 修改成功
    NET_FW_MODIFY_STATE_OK,
    # 被组策略覆盖
    NET_FW_MODIFY_STATE_GP_OVERRIDE,
    # 入站被阻止
    NET_FW_MODIFY_STATE_INBOUND_BLOCKED,
}

# 定义网络防火墙配置文件类型的枚举
public enum NET_FW_PROFILE_TYPE
{
    # 域配置文件
    NET_FW_PROFILE_DOMAIN,
    # 标准配置文件
    NET_FW_PROFILE_STANDARD,
    # 当前配置文件
    NET_FW_PROFILE_CURRENT,
    # 配置文件类型的最大值
    NET_FW_PROFILE_TYPE_MAX,
}

# 定义网络防火墙配置文件类型的另一个枚举
public enum NET_FW_PROFILE_TYPE2
{
    # 域配置文件，值为 1
    NET_FW_PROFILE2_DOMAIN = 1,
    # 私有配置文件
    NET_FW_PROFILE2_PRIVATE,
    # 公共配置文件，值为 4
    NET_FW_PROFILE2_PUBLIC = 4,
    # 所有配置文件，值为 2147483647
    NET_FW_PROFILE2_ALL = 2147483647,
}

# 定义网络防火墙规则方向的枚举
public enum NET_FW_RULE_DIRECTION
{
    # 入站规则，值为 1
    NET_FW_RULE_DIR_IN = 1,
    # 出站规则
    NET_FW_RULE_DIR_OUT,
    # 规则方向的最大值
    NET_FW_RULE_DIR_MAX,
}

# 定义网络防火墙范围的枚举
public enum NET_FW_SCOPE
{
    # 全部范围
    NET_FW_SCOPE_ALL,
    # 本地子网范围
    NET_FW_SCOPE_LOCAL_SUBNET,
    # 自定义范围
    NET_FW_SCOPE_CUSTOM,
    # 范围的最大值
    NET_FW_SCOPE_MAX,
}

# 定义网络防火墙服务类型的枚举
public enum NET_FW_SERVICE_TYPE
{
    # 文件和打印服务
    NET_FW_SERVICE_FILE_AND_PRINT,
    # UPnP 服务
    NET_FW_SERVICE_UPNP,
    # 远程桌面服务
    NET_FW_SERVICE_REMOTE_DESKTOP,
    # 无服务
    NET_FW_SERVICE_NONE,
    # 服务类型的最大值
    NET_FW_SERVICE_TYPE_MAX,
}
```