---
categories: ASP.net
---

# 控制器

当一个类后缀为Controller，那么他被视为一个控制器.  
需要调用`builder.Services.AddControllers()` 利用名字检索添加依赖注入.

## 简单的控制器

1. 控制器被注册为服务才能使用

    `builder.Services.AddControllers()`

3. 需要为控制器添加路由
    ```C#
    //老式方法
    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        //为所有控制器添加路由
        endpoints.MapControllers();
    });

    //可以用一句话代替,等价与上面
    app.MapControllers();

    ```

4. 设置方法和URL
    ```C#
    //必须为public,框架才能实例
    public class HomeController
    {
        //设置URL

        //[Route("sayhello2")] 可以同时设置多个
        //Route("contact/{mobile:int}")] 约束必须为int
        [Route("sayhello")]
        public string method1()
        {
            return "Hello from method1";
        }
    }
    ```
5. 这里已经可以使用`/sayhello`访问了,会自动调用`method1`

## 注意事项

如果控制器不想以`Controller`为后缀,那么就要写上注释`[Controller]`

## 返回值和类型

### ContentResult

```C#
public ContentResult Index()
{
    return new ContentResult()
    {Content = "Hello from Index", ContentType = "text/plain"};
}
```

如果你的控制器继承了`Controller`,那么可以用更方便的方法

```C#
public ContentResult Index()
{
    return Content("Hello from Index", "text/plain");
}
```

### JsonResult

```C#
public JsonResult Person()
{
    Person person = new Person()
    {
        Id = Guid.NewGuid(),
        FirstName = "John",
        LastName = "Doe",
        Age = 30
    };
    //return new JsonResult(person);
    return Json(person);
}
```

### FileResult

1.  VirtualFileResult

    路径以`wwwroot`开始.

    必须开启`app.UseStaticFiles()`

    ```C#
    [Route("filedownload")]
    public VirtualFileResult FileDownload()
    {
        return File("/sample", "text/plain");
        //return new VirtualFileResult("/sample","text/plain");
    }
    ```
    会直接显示在网页上.并不会下载.

1.  PhysicalFileResult

    实际的机器路径.

    ```C#
    [Route("filedownload2")]
    public PhysicalFileResult FileDownload2()
    {
        return PhysicalFile(@"/home/fs/Desktop/sample2", "text/plain");
        //return new PhysicalFileResult(@"/home/fs/Desktop/sample2","text/plain");
    }
    ```

1.  FileContentResult
    
    以数组方式返回,在复杂情况下会用到

    ```C#
    [Route("filedownload3")]
    public FileContentResult FileDownload3()
    {
        var bytes = System.IO.File.ReadAllBytes(@"/home/fs/Desktop/sample2");
        return File(bytes, "text/plain");
        //return new FileContentResult(bytes,"text/plain");
    }
    ```
### IActionResult (最常用)

为所有Result的父接口,有时一个请求在不同情况下会返回不同类型的响应.
