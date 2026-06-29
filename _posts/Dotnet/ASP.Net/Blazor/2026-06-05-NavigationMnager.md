# NavigationManager

直接从依赖注入里面得到

`_navigationManager.NavigateTo(uri)` 就能导航到目的页面

## 传入参数

`@page "/detail/{id}"`

在目的页面创建属性,并且打上标签.
```C#
public partial class Detail
{
    //这个Id属性由路由得到
    [Parameter]
    public string Id { get; set; }
}

```