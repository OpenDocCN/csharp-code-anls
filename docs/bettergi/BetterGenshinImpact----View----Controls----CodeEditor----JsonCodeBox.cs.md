# `.\better-genshin-impact\BetterGenshinImpact\View\Controls\CodeEditor\JsonCodeBox.cs`

```cs
# 导入相关的命名空间和库
﻿using BetterGenshinImpact.Helpers;
using ICSharpCode.AvalonEdit.Highlighting;
using ICSharpCode.AvalonEdit.Highlighting.Xshd;
using System.IO;
using System.Xml;

namespace BetterGenshinImpact.View.Controls.CodeEditor;

# 定义一个继承自 CodeBox 的 JsonCodeBox 类
public class JsonCodeBox : CodeBox
{
    # JsonCodeBox 构造函数
    public JsonCodeBox() : base()
    {
        # 调用注册高亮方法
        RegisterHighlighting();
    }

    # 注册 JSON 语法高亮的方法
    private void RegisterHighlighting()
    {
        # 定义一个高亮定义变量
        IHighlightingDefinition luaHighlighting;
        # 从资源中获取 JSON 语法高亮配置文件的流
        using Stream s = ResourceHelper.GetStream(@"pack://application:,,,/Assets/Highlighting/Json.xshd");
        # 使用 XML 读取器读取流中的数据
        using XmlReader reader = new XmlTextReader(s);
        # 加载高亮定义
        luaHighlighting = HighlightingLoader.Load(reader, HighlightingManager.Instance);

        # 注册 JSON 语法高亮定义
        HighlightingManager.Instance.RegisterHighlighting("Json", [".json"], luaHighlighting);
        # 设置当前的语法高亮定义
        SyntaxHighlighting = HighlightingManager.Instance.GetDefinition("Json");
    }
}
```