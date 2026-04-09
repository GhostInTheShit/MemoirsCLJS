---
categories: Avalonia
---

# 文件选择器

## OpenFilePickerAsync

打开文件

```Csharp
//从当前控件获得最其顶级控件,一般是Window页面,或者直接引用Window
var topLevel =TopLevel.GetTopLevel(this);
//异步启动文件选择器
var files=await toplevel.StorageProvider.OpenFilePickerAsync(
    //传入一个设置类
    new FilePickerOpenOptions
    {
        //设置标题
        Title="Open Text FIle",
        //是否多选
        AllowMultiple=false
    }
);

if(files.Count>=1)
{
    //获得这个文件流
    await using var stream = await files[0].OpenReadAsync();
    //接下来就是自己处理了
}

```

## SaveFilePickerAsync

保存文件