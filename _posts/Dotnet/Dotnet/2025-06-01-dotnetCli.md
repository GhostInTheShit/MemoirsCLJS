---
categories: dotnet
---

# dotnet ClI

## dotnet模板

`dotnet new <template>` 

`-f net8.0` 创建net8.0框架的模板,-f 框架,framework

`dotnet run` 运行

`--configuration Release` 在 Release 配置中运行应用程序

## dotnet包管鲤

`dotnet add package <package name>`
命令时将抓取和下载所有依赖项,默认最新版。

`--version=<version number/range>` 你可选择版本

`dotnet remove package <name of dependency>` 要移除项目中的某个包

`dotnet list package` 该命令只列出顶层包,而不列出那些包的依赖项

`--include-transitive` 列出所有可传递包

`--outdated` 列出项目中已经过时的包

`--outdated --include-prerelease` 若要检查预发行包

`dotnet restore` 在创建或克隆项目时,在生成项目之前,
将不会下载或安装随附的依赖项,
手动还原依赖项以及项目文件中指定的特定于项目的工具,
(多数情况下,不需要显式使用此命令。 如果需要, 在运行 new、build 和 run
之类的命令时,将隐式运行 NuGet 还原.)

## dotnet Tool

`dotnet tool install` 安装

`-g` 全局

`--version` 版本

>[!WARNING]
>如果出现:  
>工具的 NuGet 包中的设置文件无效: 在包中找不到设置文件 "DotnetToolSettings.xml"。  
>工具“youshow.ace.cli”安装失败。请联系工具作者获取帮助。  
>有可能是因为和你的SDK版本不匹配的原因.