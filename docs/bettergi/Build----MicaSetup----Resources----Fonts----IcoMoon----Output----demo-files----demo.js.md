# `.\better-genshin-impact\Build\MicaSetup\Resources\Fonts\IcoMoon\Output\demo-files\demo.js`

```cs
# 检查浏览器是否支持 'boxShadow' 属性，若不支持则设置 body 元素的 class 为 'noBoxShadow'
if (!('boxShadow' in document.body.style)) {
    document.body.setAttribute('class', 'noBoxShadow');
}

# 为 body 元素添加点击事件监听器
document.body.addEventListener("click", function(e) {
    # 获取事件目标元素
    var target = e.target;
    # 如果目标是 INPUT 元素且其 class 不包含 'liga'，则选中该输入框内容
    if (target.tagName === "INPUT" &&
        target.getAttribute('class').indexOf('liga') === -1) {
        target.select();
    }
});

# 自执行函数，初始化字体大小和测试文本
(function() {
    # 获取页面中的字体大小、测试驱动和测试文本元素
    var fontSize = document.getElementById('fontSize'),
        testDrive = document.getElementById('testDrive'),
        testText = document.getElementById('testText');
    
    # 更新测试区域内容
    function updateTest() {
        testDrive.innerHTML = testText.value || String.fromCharCode(160);
        # 如果定义了 icomoonLiga 函数，调用它
        if (window.icomoonLiga) {
            window.icomoonLiga(testDrive);
        }
    }
    
    # 更新测试区域的字体大小
    function updateSize() {
        testDrive.style.fontSize = fontSize.value + 'px';
    }
    
    # 监听字体大小变化，更新字体大小
    fontSize.addEventListener('change', updateSize, false);
    # 监听测试文本变化，更新测试区域内容
    testText.addEventListener('input', updateTest, false);
    # 监听测试文本内容变化，更新测试区域内容
    testText.addEventListener('change', updateTest, false);
    # 初始化字体大小
    updateSize();
}());
```