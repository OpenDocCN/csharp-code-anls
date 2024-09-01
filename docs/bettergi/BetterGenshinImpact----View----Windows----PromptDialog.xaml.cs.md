# `.\better-genshin-impact\BetterGenshinImpact\View\Windows\PromptDialog.xaml.cs`

```cs
﻿using System.Windows;
using System.Windows.Controls;

namespace BetterGenshinImpact.View.Windows;

public partial class PromptDialog
{
    // 构造函数，用于初始化对话框的各个组件
    public PromptDialog(string question, string title, UIElement uiElement, string defaultValue)
    {
        // 初始化对话框的组件
        InitializeComponent();
        // 设置对话框的标题
        MyTitleBar.Title = title;
        // 设置问题文本
        TxtQuestion.Text = question;

        // 将动态内容区域的内容设置为传入的 UI 元素
        DynamicContent.Content = uiElement;
        // 如果动态内容是文本框，则设置其默认值
        if (DynamicContent.Content is TextBox textBox)
        {
            textBox.Text = defaultValue;
        }
        // 如果动态内容是组合框，则设置其默认值
        else if (DynamicContent.Content is ComboBox comboBox)
        {
            comboBox.Text = defaultValue;
        }

        // 订阅 Loaded 事件，以便对话框加载完成后执行指定方法
        this.Loaded += PromptDialogLoaded;
    }

    // 处理对话框加载完成后的事件
    private void PromptDialogLoaded(object sender, RoutedEventArgs e)
    {
        // 将焦点设置到动态内容区域
        DynamicContent.Focus();
    }

    // 静态方法，用于创建并显示一个包含文本框的对话框
    public static string Prompt(string question, string title, string defaultValue = "")
    {
        // 创建对话框实例，并传入问题、标题、文本框和默认值
        var inst = new PromptDialog(question, title, new TextBox(), defaultValue);
        // 显示对话框并等待用户操作
        inst.ShowDialog();
        // 根据对话框的结果返回用户输入的文本或默认值
        return inst.DialogResult == true ? inst.ResponseText : defaultValue;
    }

    // 静态方法，用于创建并显示一个包含任意 UI 元素的对话框
    public static string Prompt(string question, string title, UIElement uiElement, string defaultValue = "")
    {
        // 创建对话框实例，并传入问题、标题、UI 元素和默认值
        var inst = new PromptDialog(question, title, uiElement, defaultValue);
        // 显示对话框并等待用户操作
        inst.ShowDialog();
        // 根据对话框的结果返回用户输入的文本或默认值
        return inst.DialogResult == true ? inst.ResponseText : defaultValue;
    }

    // 获取用户在对话框中输入的文本
    public string ResponseText
    {
        get
        {
            // 如果动态内容是文本框，返回其文本内容
            if (DynamicContent.Content is TextBox textBox)
            {
                return textBox.Text;
            }
            // 如果动态内容是组合框，返回其文本内容
            else if (DynamicContent.Content is ComboBox comboBox)
            {
                return comboBox.Text;
            }
            // 如果动态内容既不是文本框也不是组合框，返回空字符串
            return string.Empty;
        }
    }

    // 处理点击“确定”按钮的事件
    private void BtnOkClick(object sender, RoutedEventArgs e)
    {
        // 设置对话框结果为“确定”
        DialogResult = true;
        // 关闭对话框
        Close();
    }

    // 处理点击“取消”按钮的事件
    private void BtnCancelClick(object sender, RoutedEventArgs e)
    {
        // 关闭对话框
        Close();
    }
}
```