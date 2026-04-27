
# 依赖属性

VS `propdb`

可以在xaml中智能提示,而且只带UI渲染监听.

```cs
//有了一个公共属性，在xaml中其实就可以显示了,但是只是一个普通的.net属性，无法显示类型和智能提示
public SolidColorBrush TBrush
{
    get { return (SolidColorBrush)GetValue(TBrushProperty); }
    set { SetValue(TBrushProperty, value); }
}

//有了DependencyProperty注册，在xaml中就会通过反射提示类型.
public static readonly DependencyProperty TBrushProperty = DependencyProperty.Register(
    nameof(TBrush),
    typeof(SolidColorBrush),
    typeof(Thermometer),
    //默认值
    new PropertyMetadata(new SolidColorBrush(Colors.White))
);
```
依赖属性的changed回调。
```C#
public int CurrentValue
{
    get { return (int)GetValue(CurrentValueProperty); }
    set { SetValue(CurrentValueProperty, value); }
}

public static readonly DependencyProperty CurrentValueProperty = DependencyProperty.Register(
    nameof(CurrentValue),
    typeof(int),
    typeof(Thermometer),
    //这里可以传入回调函数
    new PropertyMetadata(0, CurrentValueChange));

//因为是在static里面注册的所以也要用static方法.
private static void CurrentValueChange(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    //static里面没法直接使用实例属性.这里提供了一个参数传递
    if (d is Thermometer thermometer)
    {
        //如果直接写这段,会报错 无法在static里面访问实例方法
        thermometer.SetBorder();
    }
}
```

## 只读依赖属性

用一层私有key保护.
```C#
public bool IsHot
{
    get => (bool)GetValue(IsHotProperty);
}

public static readonly DependencyProperty IsHotProperty = IsHotPropertyKey.DependencyProperty;

private static readonly DependencyPropertyKey IsHotPropertyKey =
    DependencyProperty.RegisterReadOnly(
        "IsHot",
        typeof(bool),
        typeof(Thermometer),
        new PropertyMetadata(false)
    );
```
