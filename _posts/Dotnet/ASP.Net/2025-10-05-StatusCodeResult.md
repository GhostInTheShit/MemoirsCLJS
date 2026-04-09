---
categories: ASP.net
---

# 状态码

```C#
//Response.StatusCode = 400;

//使用返回方法设置状态码,无响应体
return StatusCode(401);
```

## NotFoundResult

不需要返回独立的两条语句

```C#
Response.StatusCode = 400;
return Content("Something went wrong");
```

直接使用

```C#
//return new NotFoundResult();

//有参数返回值为NotFoundObjectResult,无参数为NotFoundResult,没影响
return NotFound();
```
## BadRequestResult

```C#
//return new BadRequestResult();
return BadRequest();
```
## UnauthorizedResult

```C#
//return new UnauthorizedResult();
return Unauthorized();
```

## RedirectToActionResult

重定向到控制器方法。

```C#
//Books是方法名字,Store是控制器
//return new RedirectToActionResult("Books","Store",new {id=1});
return RedirectToAction("Books","Store",new {});

//第四个参数默认为false,为true时 变为301
//return new RedirectToActionResult("Books","Store",new {},true);
return RedirectToActionPermanent("Books","Store",new {id=1});
```

>[!WARNING]
>会重新执行控制器方法,相当于重新进入了一次页面.

## LocalRedirectResult

重定向到本地Url，同样有permanent版本
```C#
//return new LocalRedirectResult(@"store/books/{id}");
return LocalRedirect(@"store/books/{id}");
```

## RedirectResult

```C#
return Redirect("www.baidu.com");
```