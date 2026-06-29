---
categories: ASP.net
---

# HttpClient

用于连接外部网页

建议使用工厂类创建,工厂类会自动管理httpclient的创建和关闭

# IHttpClientFactory

`Microsoft.Extensions.Http` 包

IHttpClientFactory 要使用容器,如果不适用容器就是一个空壳.

`serviceCollection.AddHttpClient();` 开启HttpClient功能.


## 解析

一般为JSON

VS 编辑 选择性粘贴 讲JSON粘贴为类 **属性区分大小写**