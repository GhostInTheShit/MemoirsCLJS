---
categories: ASP.net
---

# Environment 环境

Development 

Staging 预发布

Production

配置在`launchsettings.json`中


```C#
//Program.cs
//现在.net6以上已经默认添加了

//如果是开发环境
if (app.Environment.IsDevelopment())
{   
    //打开异常页面
    app.UseDeveloperExceptionPage();
}

```

## IWebHostEnvironment

`app.Environment` 里面包含很多环境的属性和设置

可以直接注入给控制器
```C#
public class HomeController : Controller
{
    private readonly IWebHostEnvironment _webHostEnvironment;

    public HomeController(IWebHostEnvironment webHostEnvironment)
    {
        _webHostEnvironment=webHostEnvironment;
    }
    // GET: HomeController
    [Route("/")]
    public ActionResult Index()
    {
        //获得环境名称
        ViewBag.CurrentEnvironment = _webHostEnvironment.EnvironmentName;
        return View();
    }
}
```

## 在特定环境下显示的视图控件 

**用的很少** 

需要这个标签助手
```C#
//_ViewImports.cshtml
@addTagHelper "*, Microsoft.AspNetCore.Mvc.TagHelpers"
```

```C#
//index.cshtml

//在Development下才会显示 这个标签在标签助手里面
<environment include="Development">//可以写exclude除了某个
    <button class="button-blue-back w-100">Button only for Development Environment</button>
</environment>
```

## 在生产环境

在生产环境中,客户机上没有`Visual Strudio`和`Rider`,而且编译出的源代码是没有`launchsettings.json`文件的.  
所以你需要使用`Power Shell`或者`Linux Bash`运行程序,代码通用`dotnet run`

  
首先进入项目根目录(不是解决方案根目录),打开`Power Shell`或者`Linux Bash`  
我们要模拟没有`launchsettings.json`文件的情况.所以必须在启动中添加`--no-launch-profile`,完整的命令为`dotnet run --no-launch-profile`



<details>
<summary>Windows PowerShell</summary>

```powershell
$Env:ASPNETCORE_ENVIRONMENT="Staging"
dotnet run --no-launch-profile
```
</details>

<details>
<summary>Linux Bash</summary>

```Bash
export ASPNETCORE_ENVIRONMENT="Staging"
dotnet run --no-launch-profile
```
</details>

以上两种命令方法都是临时设置环境,只在当前终端中有效