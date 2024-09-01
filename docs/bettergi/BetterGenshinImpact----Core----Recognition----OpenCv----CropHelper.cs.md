# `.\better-genshin-impact\BetterGenshinImpact\Core\Recognition\OpenCv\CropHelper.cs`

```cs
﻿using OpenCvSharp;
using System;

namespace BetterGenshinImpact.Core.Recognition.OpenCv;

/// <summary>
///     图片剪裁
/// </summary>
[Obsolete]
public class CropHelper
{
    // 剪裁右上角部分
    public static Mat CutRightTop(Mat srcMat, int saveRightWidth, int saveTopHeight)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从右上角开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(srcMat.Width - saveRightWidth, 0, saveRightWidth, saveTopHeight));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁右下角部分
    public static Mat CutRightBottom(Mat srcMat, int saveRightWidth, int saveBottomHeight)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从右下角开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(srcMat.Width - saveRightWidth, srcMat.Height - saveBottomHeight, saveRightWidth, saveBottomHeight));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁左上角部分
    public static Mat CutLeftTop(Mat srcMat, int saveLeftWidth, int saveTopHeight)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从左上角开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(0, 0, saveLeftWidth, saveTopHeight));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁左下角部分
    public static Mat CutLeftBottom(Mat srcMat, int saveLeftWidth, int saveBottomHeight)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从左下角开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(0, srcMat.Height - saveBottomHeight, saveLeftWidth, saveBottomHeight));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁顶部部分
    public static Mat CutTop(Mat srcMat, int saveTopHeight)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从顶部开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(0, 0, srcMat.Width, saveTopHeight));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁底部部分
    public static Mat CutBottom(Mat srcMat, int saveBottomHeight)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从底部开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(0, srcMat.Height - saveBottomHeight, srcMat.Width, saveBottomHeight));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁右侧部分
    public static Mat CutRight(Mat srcMat, int saveRightWidth)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从右侧开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(srcMat.Width - saveRightWidth, 0, saveRightWidth, srcMat.Height));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }

    // 剪裁左侧部分
    public static Mat CutLeft(Mat srcMat, int saveLeftWidth)
    {
        // 使用源图像的一部分创建新的 Mat 对象，区域从左侧开始，指定宽度和高度
        srcMat = new Mat(srcMat, new Rect(0, 0, saveLeftWidth, srcMat.Height));
        // 返回剪裁后的 Mat 对象
        return srcMat;
    }
}
```