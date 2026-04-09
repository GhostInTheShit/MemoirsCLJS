---
categories: WPF
---

# Validation

验证

ValidatesOnException

> 在属性set里面设置条件，直接抛出错误
>
> `{Binding name , ValidatesOnException=True}`
> 只要属性抛出错误就处理，并且输入框标红

ValidationRule 没用过

IDataErrorInfo 比较老了

> 在ViewModel中实现接口
>
> Error 错误汇总
>
>     public string Error
>     {
>         get
>         {
>             var errors=new List<string>
>             {
>                 this[nameof(UserName)],
>                 this[nameof(Age)]
>             };
>         }
>         return string.Join(Environment.NewLine,errors.where(x=>!string.IsNullOrEmpty(x)));
>     }
>
>     ////////////////////////////
>     //还需要在属性修改时通知Error
>     public string Name
>     {
>         get{ //... }
>         set
>         { 
>             //....
>             OnPropertyChanged(nameof(Error));
>         }
>     }
>
> this\[string columnName\] 每个属性都会执行
>
>     public string this[string columnName]
>     {
>         get
>         {
>             if(columnName==nameof(Nmae))
>             {
>                 if(string.IsNullOrEmpty(Name))
>                 return "User name is required";
>             }
>         }
>     }
>
> `{Binding name , ValidatesOnDataErrors=True}` 开启

**INotifyDataErrorInfo** 现在用的

> 三个接口成员
>
> bool HasErrors
>
> IEnumerable GetErrors()
>
> EventHandler\<DataErrorsChangedEventArgs\>? ErrorsChanged
> 前端自动绑定这个事件
>
> 主要思路是要创建一个Dictionary\<sting,List\<string\>\>来存储错误.
>
>     private readonly Dictionary<string, List<string>> _errorDictionary = new ();
>
>     public bool HasErrors => _errorDictionary.Any();
>
>     public IEnumerable GetErrors(string? property) =>
>         _errorDictionary.ContainsKey(property)? _errorDictionary[property] : null;
>
>     public void ValidateUser()
>     {
>         if (string.IsNullOrEmpty(Name))
>             AddError(Name,"不能为空");
>     }
>
>     public void AddError(string property,string errorString)
>     {
>         if (!_errorDictionary.ContainsKey(property))
>         {
>             _errorDictionary[property] = new();
>         }
>
>         if (!_errorDictionary[property].Contains(errorString))
>         {
>             _errorDictionary[property].Add(errorString);
>             OnErrorsChanged(property);
>         }
>     }
>
>     public void OnErrorsChanged(string property)
>     {
>         ErrorsChanged?.Invoke(this,new DataErrorsChangedEventArgs(property));
>     }

ObservableValidator 没看

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--如果要一启动就直接触发一次验证就在构造函数中直接赋一次空值（空白,不是null）
