# BootstrapBlazor

`BootstrapBlazor` Blazor 的 Bootstrap 风格UI

安装在官网有详细讲解.

## 控件

`BootstrapInputGroup` 标签和输入框组合
```html
<BootstrapInputGroup>
    <BootstrapInputGroupLabel DisplayText="题目"></BootstrapInputGroupLabel>
    <BootstrapInput @bind-Value="_name" ></BootstrapInput>
</BootstrapInputGroup>
```

## 遇到的问题 

如果在MAUI中使用这个库，要去除@Assets的写法.原因是ASP.net和MAUI的处理方式不同.具体暂不明白.

这是官网提供的代码
```html
<link rel="stylesheet" href="@Assets["_content/BootstrapBlazor.FontAwesome/css/font-awesome.min.css"]" />
<link rel="stylesheet" href="@Assets["_content/BootstrapBlazor/css/bootstrap.blazor.bundle.min.css"]" />
<link rel="stylesheet" href="@Assets["_content/BootstrapBlazor/css/motronic.min.css"]" />
```
这是最后使用的代码
```html
<link rel="stylesheet" href="_content/BootstrapBlazor.FontAwesome/css/font-awesome.min.css" />
<link rel="stylesheet" href="_content/BootstrapBlazor/css/bootstrap.blazor.bundle.min.css" />
<link rel="stylesheet" href="_content/BootstrapBlazor/css/motronic.min.css" />
```