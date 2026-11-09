# AttachProperty

`propa` 快捷键


```C#
internal class PasswordAttach
{
    public static string GetCustPwd(DependencyObject obj)
    {
        return (string)obj.GetValue(CustPwdProperty);
    }

    public static void SetCustPwd(DependencyObject obj, string value)
    {
        obj.SetValue(CustPwdProperty, value);
    }

    //这里注册的是附件属性
    public static readonly DependencyProperty CustPwdProperty =
        DependencyProperty.RegisterAttached("CustPwd", typeof(string), typeof(PasswordAttach), new PropertyMetadata(string.Empty));
}
```