
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
>[!warning]
>在xaml中直接赋值的依赖属性，不会经过Set,而是直接调用SetValue(dbp,value),它直接操作那个注册的依赖属性.

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

## 本地值 **(重点)** 

*   `SetCurrentValue(DependencyProperty, Object)`	  
设置依赖属性的值而不更改其值源。

*   `SetValue(DependencyProperty, Object)`  
设置依赖属性的本地值，该值由其依赖属性标识符指定。

*   `GetLocalValueEnumerator()`   
创建一个专用的枚举数，用于确定哪些依赖项属性在此 DependencyObject 上具有以本地方式设置的值。


`DependencyObject`类里面有叫本地值的东西,而`SetValue`就会将依赖属性设置为本地值，一旦有了本地值，绑定传入的值将被无视.而`SetCurrentValue`不会标记为本地值，所以不会影响绑定传入的值。

其实propdb里面也是用`SetValue`,XAML里面更是直接使用`SetValue`不经过属性.只有在XAML里面用binding才会被标记为binding赋值,不会标记为本地值    
如果你用到binding,又使用了`SetValue`就会出错

```C#
//<cp:Thermometer x:Name="MyThermometer" CurrentValue="{Binding ElementName=root,Path=Temperature}"/>

private void window_loaded()
{
    //设置了本地值
    MyThermometer.SetValue(Thermometer.CurrentValueProperty, 50);

    //这里已经没用了.绑定值已经失效了(变成代码操控了).
    Temperature = 60;
}
```