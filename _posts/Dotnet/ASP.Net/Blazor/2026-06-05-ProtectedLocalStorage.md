# ProtectedLocalStorage

Blaozr提供的浏览器LocalStorage类.

GetAsync
```C#
var result = await _localStorage.GetAsync<int>(key);
return  result.Success ? result.Value : defaultValue;
```
SetAsync
```C#
await _localStorage.SetAsync(key, value);
```