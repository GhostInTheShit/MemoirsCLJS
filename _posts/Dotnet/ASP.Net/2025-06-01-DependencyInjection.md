---
categories: ASP.net
---

# DependencyInjection

安装包 : `Microsoft.Extensions.DependencyInjection`

## 依赖注入

IServiceCollection : new一个,将类型注册进去。

```c#
//创建一个服务集合
var services = new ServiceCollection();

//注册服务
//用DefaultGreetingService来实现IGreetingService.
services.AddSingleton<IGreetingService, DefaultGreetingService>();

//IServiceProvider IServiceCollection创建一个提供者,再使用该提供者获得实例.
var serviceProvider = services.BuildServiceProvider();

//从容器中获取
var greetingService = serviceProvider.GetRequiredService<IGreetingService>();
```

ServiceDescriptor : 描述服务，没怎么用

如果已经注入SomeClass，就不需要为每个泛型(ILogger\<SomeClass\>)注入.

## 生存周期

AddTransient 瞬态 ， 无状态

AddScoped 范围内 , ASP中是每个请求为一个范围，同一个请求的多个实例为相同

AddSingleton 单例

## ASP DI

注册托管服务 :`builder.Services.AddHostedService<Worker>()`

依赖注入 :
`builder.Services.AddSingleton<IMessageWriter,MessageWriter>()`
