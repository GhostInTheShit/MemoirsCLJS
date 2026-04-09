---
categories: ASP.net
---

# HttpClient

用于连接外部网页

建议使用工厂类创建,工厂类会自动管理httpclient的创建和关闭

## 今日诗词练习


### 查看今日诗词api
获取 Token `https://v2.jinrishici.com/token`

```
//返回的 Token 在 data 中：

{
  "status": "success",
  "data": "RgU1rBKtLym/MhhYIXs42WNoqLyZeXY3EkAcDNrcfKkzj8ILIsAP1Hx0NGhdOO1I"
}
```

附带 Token 的接口,GET 方法. `https://v2.jinrishici.com/sentence`

```
//您需要在 HTTP 的 Headers 头中指定 Token 。

X-User-Token:RgU1rBKtLym/MhhYIXs42WNoqLyZeXY3EkAcDNrcfKkzj8ILIsAP1Hx0NGhdOO1I
```

在项目开始前,先GET一下查看JSON格式

### 项目设置

先去今日诗词api中获得`Token`，将它保存到`user-secrets`:

终端进入项目根目录,`dotnet user-secrets init` 初始化

再输入`dotnet user-secrets set "X-User-Token" "YourToken"`保存`Token`.

```C#
//Program.cs

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllersWithViews();
//开启HttpClient
builder.Services.AddHttpClient();
builder.Services.AddScoped<JiRiShiCiService>();
var app = builder.Build();

app.UseStaticFiles();
app.UseRouting();
app.MapControllers();
app.Run();
```

创建接口类
```C#
public interface IJiRiShiCiService
{
    //获取今日诗词
    Task<Dictionary<string,object>> GetTodayShiCi();
}
```

创建服务类
```C#
public class JiRiShiCiService :IJiRiShiCiService
{
    //使用工厂类创建,工厂类会自动管理httpclient的创建和关闭，推荐使用
    private readonly IHttpClientFactory _clientFactory;
    //使用配置类,保存敏感信息Token
    private readonly IConfiguration _configuration;

    //构造器注入
    public JiRiShiCiService(IHttpClientFactory clientFactory,IConfiguration configuration)
    {
        _clientFactory = clientFactory;
        _configuration = configuration;
    }

    //实现接口
    public async Task<Dictionary<string, object>> GetTodayShiCi()
    {
        //使用工厂类创建httpclient
        using (var httpClient = _clientFactory.CreateClient())
        {
            //创建请求
            HttpRequestMessage request = new HttpRequestMessage()
            {
                //方法
                Method = HttpMethod.Get,
                //Url
                RequestUri = new Uri("https://v2.jinrishici.com/sentence"),
            };
            //设置请求头(根据今日诗词api所说的设置请求头),从配置文件中得到
            request.Headers.Add("X-User-Token",_configuration["X-User-Token"]);
            
            //由上面请求所得到响应
            HttpResponseMessage response =await httpClient.SendAsync(request);
            
            //以流的方式读取内容
            Task<Stream> readAsStreamAsync = response.Content.ReadAsStreamAsync();
            
            StreamReader streamReader = new StreamReader(readAsStreamAsync.Result);
            
            string result = streamReader.ReadToEnd();

            //将得到的内容反序列化为字典
            var deserialize = JsonSerializer.Deserialize<Dictionary<string, object>>(result);

            if (deserialize == null)
            {
                throw new Exception("Failed to deserialize response");
            }

            if (deserialize.ContainsKey("error"))
            {
                throw new Exception($"API Error: {deserialize["error"]}");
            }

            //由于该字典有多级嵌套所以分步反序列化得到最后所需要origin(这段不太会写,只能将就实现功能)
            var data = deserialize["data"].ToString();

            var DataDic = JsonSerializer.Deserialize<Dictionary<string, object>>(data);
            
            var origin = DataDic["origin"].ToString();

            var OriginDic = JsonSerializer.Deserialize<Dictionary<string, object>>(origin);
            
            return OriginDic;
        }
    }
}
```

创建Model类

```C#
//根据每日诗词api得到的JSON所创建
public class JinRiShici
{
	public string? Title { get; set; }
	public string? Author { get; set; }
	public string? Dynasty { get; set; }
	public List<string>? Content { get; set; }
	public List<string>? Translation { get; set; }
}
```

控制器
```C#
public class HomeController : Controller
{
    private readonly JiRiShiCiService _service;

    public HomeController(JiRiShiCiService service)
    {
        _service = service;
    }
    [Route("/")]
    public async Task<ActionResult> Index()
    {
        //获得origin字典
        var OriginDic = await _service.GetTodayShiCi();
        JinRiShici jinRiShici = new JinRiShici
        {
            Title = OriginDic["title"]?.ToString(),
            Author = OriginDic["author"]?.ToString(),
            Dynasty = OriginDic["dynasty"]?.ToString(),
            //这里还有一层嵌套
            Content = JsonSerializer.Deserialize<List<string>>(OriginDic["content"]?.ToString()) ,
            //translate有些是空的
            Translation = OriginDic["translate"]==null? null:JsonSerializer.Deserialize<List<string>>(OriginDic["translate"]?.ToString()) 

        };
        return View(jinRiShici);
    }
}
```
View
```html
@model Stocks.Models.JinRiShici
@{
    ViewBag.Title = "Home";
}
<h1>@Model.Title</h1>
<h1>@Model.Dynasty @Model.Author</h1>

@foreach (var content in @Model.Content)
{
    <h1>@content</h1>
}

@if (@Model.Translation != null)
{
    <h1>翻译</h1>
    @foreach (var translation in @Model.Translation)
    {
        <h1>@translation</h1>
    }
}
```

# ~~HttpClient 老版本~~

```C#
/*
建议在应用程序生命周期中重复使用实例.

HttpClient

> BaseAddress 地址,需要一个Uri实例

HTTPContent 正文

HttpResponseMessage 响应

> IsSuccessStatusCode bool ,200-299
>
> EnsureSuccessStatusCode() 如果不成功这报错
>
> Content 响应正文

`using HttpResponseMessage response = await HttpClient.GetAsync("todos/3");`
*/
```