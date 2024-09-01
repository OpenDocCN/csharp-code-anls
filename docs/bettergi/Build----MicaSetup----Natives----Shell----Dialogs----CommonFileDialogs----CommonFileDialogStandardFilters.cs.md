# `.\better-genshin-impact\Build\MicaSetup\Natives\Shell\Dialogs\CommonFileDialogs\CommonFileDialogStandardFilters.cs`

```cs
namespace MicaSetup.Shell.Dialogs;  // 定义命名空间

#pragma warning disable CS8618  // 禁用警告 CS8618（可能未初始化的非空字段）

public static class CommonFileDialogStandardFilters  // 定义一个静态类，用于标准文件对话框过滤器
{
    private static CommonFileDialogFilter officeFilesFilter;  // 存储办公文件过滤器的静态字段
    private static CommonFileDialogFilter pictureFilesFilter;  // 存储图片文件过滤器的静态字段
    private static CommonFileDialogFilter textFilesFilter;  // 存储文本文件过滤器的静态字段

    public static CommonFileDialogFilter OfficeFiles  // 提供访问办公文件过滤器的公共属性
    {
        get
        {
            if (officeFilesFilter == null)  // 如果办公文件过滤器尚未初始化
            {
                officeFilesFilter = new CommonFileDialogFilter(LocalizedMessages.CommonFiltersOffice,  // 初始化办公文件过滤器
                    "*.doc, *.docx, *.xls, *.xlsx, *.ppt, *.pptx");  // 支持的文件扩展名
            }
            return officeFilesFilter;  // 返回办公文件过滤器
        }
    }

    public static CommonFileDialogFilter PictureFiles  // 提供访问图片文件过滤器的公共属性
    {
        get
        {
            if (pictureFilesFilter == null)  // 如果图片文件过滤器尚未初始化
            {
                pictureFilesFilter = new CommonFileDialogFilter(LocalizedMessages.CommonFiltersPicture,  // 初始化图片文件过滤器
                    "*.bmp, *.jpg, *.jpeg, *.png, *.ico");  // 支持的文件扩展名
            }
            return pictureFilesFilter;  // 返回图片文件过滤器
        }
    }

    public static CommonFileDialogFilter TextFiles  // 提供访问文本文件过滤器的公共属性
    {
        get
        {
            if (textFilesFilter == null)  // 如果文本文件过滤器尚未初始化
            {
                textFilesFilter = new CommonFileDialogFilter(LocalizedMessages.CommonFiltersText, "*.txt");  // 初始化文本文件过滤器
            }
            return textFilesFilter;  // 返回文本文件过滤器
        }
    }
}
```