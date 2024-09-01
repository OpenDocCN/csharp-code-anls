# `.\better-genshin-impact\BetterGenshinImpact\Service\Interface\IConfigService.cs`

```cs
# 引入 BetterGenshinImpact.Core.Config 命名空间，包含配置相关的类
﻿using BetterGenshinImpact.Core.Config;

# 定义命名空间 BetterGenshinImpact.Service.Interface
namespace BetterGenshinImpact.Service.Interface
{
    # 定义一个接口 IConfigService，声明配置服务的行为
    public interface IConfigService
    {
        # 获取配置，返回 AllConfig 对象
        AllConfig Get();

        # 保存配置的方法，没有返回值
        void Save();

        # 读取配置，返回 AllConfig 对象
        AllConfig Read();

        # 将配置写入，参数为 AllConfig 对象，没有返回值
        void Write(AllConfig config);
    }
}
```