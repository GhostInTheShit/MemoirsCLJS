---
categories: WPF
---

# AutoUpdater.net

`Autoupdater.NET.Official` 包


## 基本使用

客户端
```C#
//打开更新窗口,用的是系统程序集的版本
AutoUpdater.Start("http://localhost:5131/update.xml") 
```

服务器  
wwwroot  
|- update.xml 版本信息  
|- version.zip 被下载解压的文件  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<item>
	<version>1.1.0.0</version>
	<url>http://localhost:5131/version.zip</url>
	<mandatory>false</mandatory>
</item>
```