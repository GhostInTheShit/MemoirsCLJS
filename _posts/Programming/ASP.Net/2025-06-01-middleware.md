---
categories: ASP.net
---

# 中间件

是ASP.NET Core
中的核心组件，MVC,响应缓存,身份验证,CORS,Swagger都是内置中间件

执行顺序  
middleware 1->middleware 2->middleware 3->middleware 2->middleware 1



## Base

**仅在受到请求时执行**

### app.Use()

使用中间件

```C#
app.Use(async (HttpContext context, RequestDelegate next) =>
{
    //before logic  
    await next(Context);//可以不调用  
    //after logic  
});
```

>[!Warning]
>如果写成
>app.Use(context,next)  
>那么一定要写await next(Context);因为Use有多个重载，它无法推断是哪个,写了他就能推断。

**可以选择终止和继续发送到下一个中间件**

### app.Run() 

终结中间件

**不会将请求转发到后续中间件** ,会在`app.Use()`执行完后执行,不像是个中间件,而是个终结件.

有一个参数的委托，参数是一个 HttpContext 对象 并且返回 Task

app.UseRewriter(new RewriteOptions().AddRedirect(\"history\",\"about\"))  
将histroy 重定向到 about **重定向会发送一个新的请求**

### app.UseWhen()

满足条件下,开启的中间件

```C#
app.UseWhen(
    //条件
    context => context.Request.Query.ContainsKey("username"), 
    //开启中间件.
    app=>
    {
        app.Use(async (context,next) =>
        {
            await context.Response.WriteAsync("hello from middleware branch.");
            await next();
        });
    });
```


## 自定义中间件

``` CSharp
/// Program.cs
/// 启动中间件
app.UseMiddleware<>();
```

### 接口方法(老方法，现不推荐使用)

``` C#
public class MyCustomMIddleware : IMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        await context.Response.WriteAsync("Hello from Custom Middleware\n");
        await next(context);
        await context.Response.WriteAsync("Goodbye from Custom Middleware\n");
    }
}

//创建扩展
public static class CustomMiddlewareExtension
{
    public static IApplicationBuilder UseMyCustomMIddleware(this IApplicationBuilder app)
    {
        return app.UseMiddleware<MyCustomMIddleware>();
    }
}
```

### 普通类方法(主流方法)

```C#
//直接构造器接收next,方法接收context.
public class HelloCustomMiddleware
{
    private readonly RequestDelegate _next;

    public HelloCustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        if (context.Request.Query.ContainsKey("firstname")&& context.Request.Query.ContainsKey("lastname"))
        {
            string fullName= context.Request.Query["firstname"]+""+ context.Request.Query["lastname"];
            await context.Response.WriteAsync(fullName);
        }
        await _next(context);
    }
}

//创建扩展
public static class HelloCustomMiddlewareExtensions
{
    public static IApplicationBuilder UseHelloCustomMiddleware(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<HelloCustomMiddleware>();  
    }
}
```

### 中间件启用顺序

```
Request
   ↓
[ExceptionHandler]        → 捕获全局异常
   ↓
[HSTS]                    → 强制浏览器用 HTTPS
   ↓
[HttpsRedirection]        → HTTP → HTTPS 重定向
   ↓
[StaticFiles]             → 静态文件直接返回（如 .js, .css, .png）
   ↓
[Routing]                 → 解析 URL，匹配到对应 Endpoint
   ↓
[CORS]                    → 设置跨域策略
   ↓
[Authentication]          → 验证用户身份（如 JWT、Cookie）
   ↓
[Authorization]           → 检查是否有权限访问
   ↓
[Custom Middleware 1]     → 自定义中间件（如日志、性能监控）
   ↓
[Custom Middleware 2]     → 可多个，按注册顺序执行
   ↓
[Endpoint]                → 最终处理（Controller / Minimal API）

```

## 异常中间件 

UseExceptionHandler

```C#
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
else
{   
    //出现了错误就显示该页面
    app.UseExceptionHandler("/Home/Error");
}
```

```C#
[Route("[action]")]
public ActionResult Error()
{
    //在httpcontext.Features查看是否有异常
    var exceptionHandlerPathFeature = HttpContext.Features.Get<IExceptionHandlerPathFeature>();
    if (exceptionHandlerPathFeature!=null&&exceptionHandlerPathFeature.Error!=null)
    {
        //有的话就设置ViewData.
        ViewBag.ErrorMessage=exceptionHandlerPathFeature.Error.Message;
    }
    return View();
}
```



### 自定义异常中间件 
自定义异常中间件,不过不太使用,因为内置的已经够好了.(跟着教程写的时候发现结果不能实现)
```C#
public class ExceptionHandingMiddleware
{
    private readonly RequestDelegate _next;

    public ExceptionHandingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext context)
    {
        try
        {
            await _next(context); 
        }
        catch (Exception e)
        {
            Console.WriteLine(e);
            throw;
        }
    }
}
```