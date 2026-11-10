# AttachProperty

`propa` 快捷键

不在自己的控件上,有点像扩展方法

```C#
public class PasswordAttach
{
    #region CustPwd Changed PasswordBox Password Changed
    public static string GetCustPwd(DependencyObject obj)
    {
        return (string)obj.GetValue(CustPwdProperty);
    }

    public static void SetCustPwd(DependencyObject obj, string value) 
    {
        obj.SetValue(CustPwdProperty, value);
    }

    //注册附加属性
    public static readonly DependencyProperty CustPwdProperty =
        DependencyProperty.RegisterAttached("CustPwd", typeof(string), typeof(PasswordAttach), new PropertyMetadata(string.Empty,OnCustPwdChanged));

    //附加属性变了就修改目标属性
    private static void OnCustPwdChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        string? v = e.NewValue?.ToString();
        if(d is PasswordBox passwordBox)
        {
            passwordBox.Password = v ?? string.Empty;
        }
    }
    #endregion

    #region Password Changed CustPwd Changed
    public static bool GetIsOpen(DependencyObject obj)
    {
        return (bool)obj.GetValue(IsOpenProperty);
    }

    public static void SetIsOpen(DependencyObject obj, bool value)
    {
        obj.SetValue(IsOpenProperty, value);
    }

    //默认值为false,在Xaml中设置为true,触发OnIsOpenChanged
    public static readonly DependencyProperty IsOpenProperty =
        DependencyProperty.RegisterAttached("IsOpen", typeof(bool), typeof(PasswordAttach), new PropertyMetadata(false,OnIsOpenChanged));

    //订阅方法
    private static void OnIsOpenChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        if(d is PasswordBox passwordBox)
        {
            passwordBox.PasswordChanged -= PasswordBox_PasswordChanged;
            passwordBox.PasswordChanged += PasswordBox_PasswordChanged;
        }
    }

    //值一改变就修改附加属性的值
    private static void PasswordBox_PasswordChanged(object sender, RoutedEventArgs e)
    {
        if (sender is PasswordBox passwordBox)
        {
            SetCustPwd(passwordBox, passwordBox.Password);
        }
    }
    #endregion
}
```