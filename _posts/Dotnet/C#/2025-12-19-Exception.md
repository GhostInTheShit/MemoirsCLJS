---
categories: C#
---

# Exception

异常

## 自定义异常

自定义更精确的异常,方便查看.


至少要有三种构造函数
```C#
public class InvalidPersonIDException : ArgumentException
{
    public InvalidPersonIDException() : base()
    {
    }

    public InvalidPersonIDException(string message) : base(message)
    {
        
    }

    public InvalidPersonIDException(string message, Exception inner) : base(message, inner)
    {
        
    }
}
```
其实就是换了个名字.