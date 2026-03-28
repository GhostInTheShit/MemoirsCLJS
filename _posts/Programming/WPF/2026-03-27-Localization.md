
# 本地化

## CultureInfo

用来格式化的

```C#
MyDate.ToString(new CultureInfo("en-US"));
```

### CurrentCulture,CurrentUICulture 

可以在OnStarup中更改区域文化,已经生成的值不会自动更新.

```C#
//老版本，改变当前线程区域
Thread.CurrentThread.CurrentCulture = new CultureInfo("de-DE");
Thread.CurrentThread.CurrentUICulture = new CultureInfo("en-US");

//新版本，改变默认线程区域,之后的线程也会基于这个区域
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("zh-CN");
CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("zh-CN");
```

