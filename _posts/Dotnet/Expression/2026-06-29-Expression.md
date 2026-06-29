
# Expression

ParameterExpression 表达式参数，指lambda的参数

MemberExpression 成员参数,指谁的属性或字段,一般用来得到表达式参数的成员

## 方法

### 静态方法
`Expression.Parameter`

Expression.Parameter(typeof(User), "x");

生成lambda参数,类似`(x)=>{}`的x;

`Expression.Property`

Expression.Property(parameter,PropertyName);

指parameter参数里面的PropertyName属性

`Expression.Constant`

Expression.Constant(Content,typeof(string));

指为Content的string常数

`Expression.Call`

Expression.Call(property,method,condition);     

为property属性调用method,参数为condition

`Expression.Lambda`

```C#
//封装表达式,形成一个真正的表达式.
//返回值为Expression<Func<Poetry, bool>> 
//第一个参数为表达体,第二个为参数
Expression.Lambda<Func<Poetry, bool>>(aggregatedExpression, parameter);
```

### 非静态方法

`Aggregate()` 聚合

Aggregate(Expression.Constant(true) as Expression, Expression.AndAlso)

第一个参数为初始值,第二个参数指&&,也就是且真的聚合
        

