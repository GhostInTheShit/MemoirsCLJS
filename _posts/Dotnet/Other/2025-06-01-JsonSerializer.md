---
categories: dotnet
---

# Json序列号和反序列化

System.Text.Json

JsonSerializer 能将C#对象转换为Json

JsonDocument 用于读取和解析Json数据的类

JsonElement 表示Json的结构体

**默认情况只会序列号属性而不序列号字段**

------------------------------------------------------------------------

## 序列化

JsonSerializer.Serialize(obj) 将一个对象序列号

`JsonSerializerOptions` 可选参数

属性标记

`[JsonIgnore]`

`[JsonIgnore(Condition=JsonIgnoreCondition.Never)]`

## 反序列化

`JsonSerializer.DeSerialize<T>(jsonString)` 反序列化 如果失败则为null


## JsonSerializerOptions

MaxDepth 最大深度

ReferenceHandler 引用处理,如果两个类互相引用,父子关系.可能会无限循环

```c#
var options = new JsonSerializerOptions
{
    //忽略只读属性
    IgnoreReadOnlyProperties = true,
    //忽略null值属性,
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,,
    //忽略默认类型
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingDefault,
    //包括字段,默认不序列号字段
    IncludeFields = true,
};
jsonString = JsonSerializer.Serialize(weatherForecast, options);
```

## DTO Data Transfer Objects

其实就是创建了一个副本类，简化序列化，也方便自定义操作,

如果源本类某些设置不支持序列化,则创造DTO类序列化.

```C#
public class BankCustomerDTO
{
    public string CustomerId { get; set; } = "";
    public string FirstName { get; set; } = "";
    public string LastName { get; set; } = "";
    public List<int> AccountNumbers { get; set; } = new List<int>();

    public static BankCustomerDTO MapToDTO(BankCustomer bankCustomer)
    {
        return new BankCustomerDTO
        {
            CustomerId = bankCustomer.CustomerId,
            FirstName = bankCustomer.FirstName,
            LastName = bankCustomer.LastName,
            AccountNumbers = bankCustomer.Accounts.Select(a => a.AccountNumber).ToList()
        };
    }
}
```