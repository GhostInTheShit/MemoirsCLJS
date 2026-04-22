---

---

# 配置

配置是只读的.

* appsettings.{Environment}.json
* Environment variables
* Command-line arguments
* Other sources

## 手动创建

**不详细讲,基本用依赖注入**

`Microsoft.Extensions.Configuration` 包，不是所有模板项目都自带配置功能

## 依赖注入

`using IHost host = Host.CreateApplicationBuilder(args).Build()`
自动添加配置功能.使用`IConfiguration`接收

优先级从上到下
*   使用命令行配置提供程序通过命令行参数提供。
*   使用环境变量配置提供程序来配置环境变量。
*   在环境中运行时的Development。
*   使用 JSON 配置提供程序通过 Environment.json 提供。 例如，appsettings.Production.json 和 appsettings.Development.json。
*   使用 JSON 配置提供程序通过 appsettings.json 提供。
*   ChainedConfigurationProvider ：添加现有 IConfiguration 作为源。

如果项目根目录没有`appsettings.json`就创建一个,只要有了host它会自动查找这个文件并且解析。

## 用数据库连接字符串举例

appsettings.json
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=ADOLearnDB;Integrated Security=True;"
  }
}
```
使用配置的类
```cs
private readonly IConfiguration configuration;

public CRUDBase(IConfiguration configuration)
{
    this.configuration = configuration;
}

public string ConnString => configuration.GetValue<string>("ConnectionStrings:DefaultConnection");
```

## 绑定

指如何用.net对象得到配置文件里面的值。

```json
{
    "KeyOne": 1,
    "KeyThree": {
        "Message": "Thanks for checking this out!"
    }

    "SupportedVersions": {
        "v1": "1.0.0",
        "v3": "3.0.7"
    },

    "IPAddressRange": [
        "46.36.198.123",
        "46.36.198.124",
        "46.36.198.125"
    ]
}
```

```cs
config.GetValue<int>("KeyOne")

//如果有嵌套
config.GetValue<string>("KeyThree:Message");
//另一种写法
config["SupportedVersions:v1"];

//如果是数组
config["IPAddressRange:0"];
```