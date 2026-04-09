---
categories: ASP.net
---

# FluentAPI

是EFCORE里面的内置方法,因为是数据库设计,需要和迁移一起使用.

```C#
//SomethingDbContext.cs
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    //Fluent API,Entity级别
    modelBuilder.Entity<Person>().Property(p => p.TIN)
        .HasColumnName("TaxIdentificationNumber")
        .HasColumnType("varchar(8)")
        .HasDefaultValue("ABC1234");

    //HasIndex添加索引能提高查询速度,但是降低插入等速度
    //IsUnique唯一,和上面默认值冲突,这里仅作参考
    modelBuilder.Entity<Person>().HasIndex(p => p.TIN).IsUnique();
}
```

## 表关系
```C#
//SomethingDbContext.cs
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    //Table Relations,一对多,
    modelBuilder.Entity<Person>(p =>
    {
        //Person有一个Country
        p.HasOne<Country>(p => p.Country)
            //并且Country有很多Person
            .WithMany(c => c.Persons)
            //外键为Person的CountryId
            .HasForeignKey(p=>p.CountryId);
    });
}
```















