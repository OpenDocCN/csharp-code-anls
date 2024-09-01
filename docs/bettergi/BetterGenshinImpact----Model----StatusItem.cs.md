# `.\better-genshin-impact\BetterGenshinImpact\Model\StatusItem.cs`

```cs
# 引入 CommunityToolkit.Mvvm.ComponentModel 命名空间，提供 MVVM 相关功能
using CommunityToolkit.Mvvm.ComponentModel;
# 引入系统命名空间
using System;
# 引入组件模型命名空间，提供属性更改通知功能
using System.ComponentModel;

# 定义命名空间 BetterGenshinImpact.Model
namespace BetterGenshinImpact.Model
{
    # 定义 StatusItem 类，继承 ObservableObject，支持属性通知
    public partial class StatusItem : ObservableObject
    {
        # 定义 Name 属性，公开的字符串类型
        public string Name { get; set; }
        # 定义 _sourceObject 私有字段，类型为 INotifyPropertyChanged
        private INotifyPropertyChanged _sourceObject { get; set; }
        # 定义 _propertyName 私有字段，存储属性名称的字符串
        private string _propertyName { get; set; }

        # 使用 ObservableProperty 特性生成 IsEnabled 属性，私有字段自动生成
        [ObservableProperty] private bool _isEnabled;

        # StatusItem 构造函数，初始化 Name、_sourceObject 和 _propertyName
        public StatusItem(string name, INotifyPropertyChanged sourceObject, string propertyName = "Enabled")
        {
            Name = name;  # 设置 Name 属性
            _sourceObject = sourceObject;  # 设置 _sourceObject 字段
            _propertyName = propertyName;  # 设置 _propertyName 字段

            # 订阅 sourceObject 的 PropertyChanged 事件，以便在属性变化时更新 IsEnabled
            _sourceObject.PropertyChanged += OnSourcePropertyChanged;
            # 初始化 IsEnabled 属性的值
            IsEnabled = GetSourceValue();
        }

        # 获取源对象指定属性的布尔值
        private bool GetSourceValue()
        {
            # 获取源对象类型的指定属性
            var property = _sourceObject.GetType().GetProperty(_propertyName);
            # 如果属性为空，抛出异常
            ArgumentNullException.ThrowIfNull(property);
            # 获取该属性的值
            var value = property.GetValue(_sourceObject);
            # 如果值为空，抛出异常
            ArgumentNullException.ThrowIfNull(value);
            # 将值转换为布尔型并返回
            return (bool)value;
        }

        # 处理源对象属性变化事件
        private void OnSourcePropertyChanged(object? sender, PropertyChangedEventArgs e)
        {
            # 如果变化的属性是指定的属性名称
            if (e.PropertyName == _propertyName)
            {
                # 更新 IsEnabled 属性的值
                this.IsEnabled = GetSourceValue();
            }
        }
    }
}
```