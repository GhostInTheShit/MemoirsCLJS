---
categories: ASP.net
---

# ASP.NET Core Razor Pages

## 入门

1.  创建模型类

    > 创建Models文件夹，在里面创建模型类Movies.cs

2.  搭建基架

    > 在Pages中创建Movies文件夹，添加 `scaffolded item` 选择Razor(CRUD).
    >
    > 会自动创建以下:
    >
    > Pages/Movies："创建"、"删除"、"详细信息"、"编辑"和"索引"。
    >
    > Data/RazorPagesMovieContext.cs

3.  迁移和更新

## 项目文件

- .cshtml 包含使用Razor语法的C#代码的HTML
- .cshtml.cs 其中包含处理页面事件的C#代码

[Layout.cshtml]{#layout.cshtml} 文件可配置所有页面通用的 UI 元素.

wwwroot文件夹: 包含静态资产，如 HTML 文件、JavaScript 文件和 CSS 文件.

appsettings.json 包含配置数据, **连接sqlserver的凭证就写在这里**

使用的是LocalDb

> SQL Server Express LocalDB
>
> LocalDB是轻型版的SQL Server Express 数据库引擎，以程序开发为目标.
>
> LocalDB 作为按需启动并在用户模式下运行的轻量级数据库没有复杂的配置.
>
> 默认情况下，LocalDB 数据库在C:Users\<user\>目录下创建 `*.mdf` 文件。

## 模型绑定

`[BindProperty(SupportsGet = true)]` 可以让Get页面上接受属性参数.

    [BindProperty(SupportsGet = true)]
    public string? SearchString { get; set; }

    //https://localhost:5001/Movies?searchString=Ghost

`@page "{searchString?}"` 设置路由模板,可以直接
`https://localhost:5001/Movies/Ghost`

## 模型的改变

如果在开发中model类发生改变.由三种方法解决:重新创建,改表.迁移.

- 让 Entity Framework 自动丢弃并使用新的模型类架构 **重新创建** 数据库。
  此方法在开发周期早期很方便，开发人员可通过它一起快速改进模型和数据库架构。
  它的缺点是 **数据库中的现有数据会丢失** 。
  请勿对生产数据库使用此方法！
  在架构更改时丢弃数据库，并使用初始化表达式通过测试数据自动设定数据库种子，这通常是开发应用的有效方式。
- 对现有数据库架构进行显式 **修改** ，使它与模型类相匹配。
  此方法的优点是可以保留数据。
  可以手动或通过创建数据库更改脚本进行此更改。
- 使用 EF Core **迁移** 更新数据库架构。
- EF Core 更新数据库
- 要运行一下数据库才会更新

## 验证

数据验证，优势是不需要在页面中更改代码。

在model的属性上面注释。

\[Required\]属性必须有一个值.不阻止用户输入空格来满足此验证

\[RegularExpression\] 正则表达式，

\[Range\] 将值限定在范围内

\[stringLength\] 字符串属性的最大长度，以及可选的最小长度

> \[MinimumLength\]

在输入错误时候，会自动在网页上出现错误提示.存在客户端验证错误时，不会将表单数据发布到服务器。

在浏览器中禁用 JavaScript
后，提交出错表单将发布到服务器。但是服务器仍然有验证(`ModelState.IsValid`)。

这种验证的方式好处在于只要在Model中添加规则，而不需要在多个页面中一一更改。非常方便简洁.

## Razor和EF Core

待续
