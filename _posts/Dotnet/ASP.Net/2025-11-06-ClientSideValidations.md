---
categories: ASP.net
---

# 客户端验证

在客户端进行验证如果不过,将不会发送请求.

需要 数据注解(DTO验证规则)，数据存储属性(asp标签助手)和导入JQuery,它就会自动有效.

导入`jquery` `jquery-validate` `jquery-validation-unobtrusive` 必须按顺序

```Html
//这里把它写在section里面
//在布局里面@RenderSection("scripts", required: false);

@section scripts
{
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js" 
            integrity="sha512-894YE6QWD5I59HgZOGReFYm4dnWc1Qt5NtvYSaNcOP+u1T9qYdvdihz0PPSiiqn/+/3e7Jo4EaG7TubfWGUrMQ==" 
            crossorigin="anonymous" 
            referrerpolicy="no-referrer"
            asp-fallback-test="Window.jQuery" 
            asp-fallback-src="~/jquery.min.js"></script>
    <!--除了asp标签其他的都是cdn link自带的-->
    <!--asp-fallback-test="Window.jQuery" 是固定的,指电脑会测试这个cdn-->
    <!--asp-fallback-src="~/jquery.min.js" 源文件,如果测试到有问题会使用源文件,~是wwwroot-->

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.4/jquery.validate.min.js" 
            integrity="sha512-FOhq9HThdn7ltbK8abmGn60A/EMtEzIzv1rvuh+DqzJtSGq8BRdEN0U+j0iKEIffiw/yEtVuladk6rsG4X6Uqg==" 
            crossorigin="anonymous" 
            referrerpolicy="no-referrer"
            asp-fallback-test="Window.jQuery.validator" 
            asp-fallback-src="~/jquery.validate.min.js"></script>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validation-unobtrusive/3.2.12/jquery.validate.unobtrusive.min.js" 
            integrity="sha512-o6XqxgrUsKmchwy9G5VRNWSSxTS4Urr4loO6/0hYdpWmFUfHqGzawGxeQGMDqYzxjY9sbktPbNlkIQJWagVZQg==" 
            crossorigin="anonymous" 
            referrerpolicy="no-referrer"
            asp-fallback-test="Window.jQuery.validate.unobtrusive" 
            asp-fallback-src="~/jquery.validate.unobtrusive.min.js"></script>
}
```

但是开启后，验证错误将不会经过服务器，那么就无法得到错误消息.  

加入以下代码到前端,asp辅助标签将自动添加错误信息

```html
<!--可以写在输入框下面-->
<span asp-validation-for="PersonName"></span>

<!--写在提交按钮下面,消息汇总-->
<div asp-validation-summary="All"></div>
```

asp帮助标签会得到模型注释的信息,Jquery会和asp联动

