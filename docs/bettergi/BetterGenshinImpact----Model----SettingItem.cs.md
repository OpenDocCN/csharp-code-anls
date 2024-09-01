# `.\better-genshin-impact\BetterGenshinImpact\Model\SettingItem.cs`

```cs
# 引入必要的系统库和控件库
﻿using System;
using System.Collections.Generic;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;
using System.Windows.Data;
using System.Xml.Linq;

# 定义命名空间
namespace BetterGenshinImpact.Model;

# 标记类为可序列化
[Serializable]
public class SettingItem
{
    # 定义设置项的名称，初始化为空字符串
    public string Name { get; set; } = string.Empty;
    # 定义设置项的类型，初始化为空字符串
    public string Type { get; set; } = string.Empty;
    # 定义设置项的标签，初始化为空字符串
    public string Label { get; set; } = string.Empty;

    # 定义设置项的选项列表，可能为 null
    public List<string>? Options { get; set; }

    # 定义设置项的默认值，可能为 null
    public object? Default { get; set; }

    # 根据设置项信息生成控件
    public List<UIElement> ToControl(dynamic context)
    {
        # 创建控件列表
        var list = new List<UIElement>();

        # 创建标签控件，设置其内容和边距
        var label = new Label
        {
            Content = Label,
            Margin = new Thickness(0, 0, 0, 5)
        };
        # 将标签添加到控件列表
        list.Add(label);

        # 创建数据绑定对象，绑定到指定的上下文和属性路径
        var binding = new Binding
        {
            Source = context,
            Path = new PropertyPath(Name)
        };
        # 根据设置项的类型生成不同的控件
        switch (Type)
        {
            case "input-text":
                # 创建文本框控件，设置其名称和边距
                var textBox = new TextBox
                {
                    Name = Name,
                    Margin = new Thickness(0, 0, 0, 10)
                };
                # 如果有默认值，设置文本框的文本
                if (Default != null)
                {
                    textBox.Text = Default.ToString()!;
                }
                # 将数据绑定设置到文本框的 Text 属性
                BindingOperations.SetBinding(textBox, TextBox.TextProperty, binding);
                # 将文本框添加到控件列表
                list.Add(textBox);
                break;

            case "select":
                # 创建组合框控件，设置其名称和边距
                var comboBox = new ComboBox
                {
                    Name = Name,
                    Margin = new Thickness(0, 0, 0, 10)
                };
                # 如果有选项，添加到组合框中
                if (Options != null)
                {
                    foreach (var option in Options)
                    {
                        comboBox.Items.Add(option);
                    }
                }
                # 如果有默认值，设置组合框的选中项
                if (Default != null)
                {
                    comboBox.SelectedItem = Default;
                }
                # 将数据绑定设置到组合框的 ItemsSource 属性
                BindingOperations.SetBinding(comboBox, ItemsControl.ItemsSourceProperty, binding);
                # 将组合框添加到控件列表
                list.Add(comboBox);
                break;

            case "checkBox":
                # 创建复选框控件，设置其名称和边距
                var checkBox = new CheckBox
                {
                    Name = Name,
                    Margin = new Thickness(0, 0, 0, 10)
                };
                # 如果有默认值，设置复选框的选中状态
                if (Default != null)
                {
                    checkBox.IsChecked = (bool)Default;
                }
                # 将数据绑定设置到复选框的 IsChecked 属性
                BindingOperations.SetBinding(checkBox, ToggleButton.IsCheckedProperty, binding);
                # 将复选框添加到控件列表
                list.Add(checkBox);
                break;

            default:
                # 如果设置项类型未知，抛出异常
                throw new Exception($"Unknown setting type: {Type}");
        }
        # 返回控件列表
        return list;
    }
}
```