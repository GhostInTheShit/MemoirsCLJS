---
categories: Asp.net
---

# 静态文件

在根目录创建一个文件夹`wwwroot`,将静态文件放入其中,则可以直接用Url访问.

## 修改wwwrooot名称

```C#
//修改文件夹名称
var builder = WebApplication.CreateBuilder(new WebApplicationOptions()
{
    WebRootPath = "myroot"
});
```

但还是最好创建`wwwroot`,因为如果程序没有找到自定义文件则会报错. 

## 管理多个文件夹(实际中很少使用)

```C#
app.UseStaticFiles(new StaticFileOptions()
{   
    //builder.Environment.ContentRootPath为项目路径
    FileProvider = new PhysicalFileProvider(Path.Combine(builder.Environment.ContentRootPath,"myroot"))
});
```
它会优先查找wwwroot中的静态文件,再查找myroot。
