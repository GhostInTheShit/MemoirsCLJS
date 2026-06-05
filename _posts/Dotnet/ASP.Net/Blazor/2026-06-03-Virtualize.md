# Virtualize

虚拟滚动控件,页面其实只有那么几十条数据，UI复用，数据更改，展现出一种很多条数据的观感。性能很好。

傻瓜化控件，你只需要告诉它总数据为多少条，关于每次加载多少条，已经加载了多少条都是它自己计算和提供的.

razor
```html
<!-- ItemsProvider为按需加载的方法,也可以直接使用Items一次性加载，二者不能同时使用 -->
<!-- ItemSize默认为50,可以按F12查看实际为多少 -->
<Virtualize ItemsProvider="LoadMoreAsync" ItemSize="40">
    <p>@context.Name @context.Author @context.Id</p>
</Virtualize>
```
C#
```C#
//request是它会提供给你的参数，里面StartIndex和Count都是它自己管理的.在加载时使用它给你的参数，不使用它的UI计算就会乱掉
private async ValueTask<ItemsProviderResult<Poetry>> LoadMoreAsync(ItemsProviderRequest request)
{
    IEnumerable<Poetry> poetries = await _poetryStorage.GetPoetriesAsync(_where,request.StartIndex, request.Count);
    //这里30为总数据数量(你数据库里的总数据量).
    return new ItemsProviderResult<Poetry>(poetries,30);
}
```