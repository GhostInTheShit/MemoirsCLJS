# TransactionScope

事务回滚

```C#
[HttpPost]
public string Test1()
{
    
    dbContext.Books.Add(new Book { Name = "Book1", Price = 100 });
    dbContext.Persons.Add(new Person { Name = "Person1", Age = 30 });
    //这样写本身就是一个事务,如果有报错则不会保存数据(就是回滚)
    dbContext.SaveChanges(); 
    return "Ok";
    
}
```
`TransactionScope`
```C#
//这里用SaveChanges()将两个数据单独保存模拟两个独立的业务
//则使用Transactionscope包裹成为一个单独事务,报错则自动回滚
[HttpPost]
public string Test1()
{
    using (TransactionScope tx = new TransactionScope())
    {
        dbContext.Books.Add(new Book { Name = "Book1", Price = 100 });
        dbContext.SaveChanges();

        dbContext.Persons.Add(new Person { Name = "Person1", Age = 30 });
        dbContext.SaveChanges(); //事务

        tx.Complete();
        return "Ok";
    }
}
```

在异步中要传入`TransactionScopeAsyncFlowOption.Enabled`,否则会报错
```C#
[HttpPost]
public async Task<string> Test1()
{
    using (TransactionScope tx = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
    {
        dbContext.Books.Add(new Book { Name = "Book1", Price = 100 });
        await dbContext.SaveChangesAsync();
        dbContext.Persons.Add(new Person { Name = "Person1", Age = 30 });
        await dbContext.SaveChangesAsync(); //事务
        tx.Complete();
        return "Ok";
    }
}
```

## 练习
默认所有Action启动事务,`NotTransationAttribute`标记的不启动事务

创建属性类
```C#
public class NotTransationAttribute : Attribute { }
```

ActionFilter
```C#
public class TransationScopeFilter : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(
        ActionExecutingContext context,
        ActionExecutionDelegate next
    )
    {
        //转化为ControllerActionDescription 可以得到更多信息,这里需要的是MethodInfo
        ControllerActionDescriptor? controllerActionDescriptor =
            context.ActionDescriptor as ControllerActionDescriptor;

        //isTx action是否要启动事务
        bool isTx = false;

        //如果为空 方法直接结束
        if (controllerActionDescriptor != null)
        {
            //获取这个action上面是否存在`NotTransationAttribute`标记
            bool hasNotTransationAttribute = controllerActionDescriptor
                .MethodInfo.GetCustomAttributes(typeof(NotTransationAttribute), false)
                .Any();
            //有`NotTransationAttribute`则不启动事务
            isTx = !hasNotTransationAttribute;
        }
        //如果需要启动事务
        if (isTx)
        {
            //包裹TransactionScope，传入异步设置
            using (var tx = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {

                var r = await next();
                //如果有异常则不提交
                if (r.Exception == null)
                {
                    tx.Complete();
                }
            }
        }
        //不启动事务直接next
        else
        {
            await next();
        }
    }
}
```
注册添加filter
```C#
builder.Services.Configure<MvcOptions>(options =>
{
    options.Filters.Add<TransationScopeFilter>();
});
```