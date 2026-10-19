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
        return "Ok";
    }
}
```