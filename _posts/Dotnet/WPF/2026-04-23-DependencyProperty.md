
# 依赖属性

VS `propdb`

可以在xaml中智能提示,而且只带UI渲染监听.

支持绑定

自定义 扩展控件属性
 
验证 回调

附加属性(特殊的依赖属性)

```cs
public class MyDependencyOb : UIElement
{
    //包装
    public int Age
    {
        get { return (int)GetValue(AgeProperty); }
        set { SetValue(AgeProperty, value); }
    }

    //声明,注册
    public static readonly DependencyProperty AgeProperty =
        //注册名(Xaml中的属性名),属性的类型,属于谁,[optional] 元素据(默认值,值改变回调,强制回调),[optional] 验证回调
        DependencyProperty.Register(nameof(Age), typeof(int), typeof(MyDependencyOb), new PropertyMetadata(0,OnAgeChangedBack,OnCoerceBack),new ValidateValueCallback(OnValidAgeBack));

    //属性值改变回调,三个回调只有这个常用
    private static void OnAgeChangedBack(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        Console.WriteLine($"Age changed from {e.OldValue} to {e.NewValue}");
    }

    //强制回调
    private static object OnCoerceBack(DependencyObject d, object baseValue)
    {
        int age = (int)baseValue;
        if (age == 100) return 95;
        return age;
    }

    //验证回调
    //在Xaml中就会提示正确与否
    private static bool OnValidAgeBack(object value)
    {
        int age = (int)value;
        if (age > 200) return false;
        return true;
    }
}
```

>[!Warning]
>框架设置依赖属性的时候,直接调用DependencyObject.SetValue(),不经过自己写的set{}访问器.

```C#
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

## 依赖继承

父类注册,子类添加owner.  

`PropertyMetadata`换成`FrameworkPropertyMetadata`  


```C#
//父类
public class InheritedDependencyOb : Panel
{
    public string MyName
    {
        get { return (string)GetValue(MyNameProperty); }
        set { SetValue(MyNameProperty, value); }
    }

    //注册依赖属性,最后一个元数据填FrameworkPropertyMetadata
    public static readonly DependencyProperty MyNameProperty =
        DependencyProperty.Register(nameof(MyName), typeof(string), typeof(InheritedDependencyOb), new FrameworkPropertyMetadata("", FrameworkPropertyMetadataOptions.Inherits));
}
```

```C#
//子类
public class InheritDependencyOb : Panel
{
    public string MyName
    {
        get { return (string)GetValue(MyNameProperty); }
        set { SetValue(MyNameProperty, value); }
    }

    //挂载到父类的依赖属性上(在父类的依赖属性上添加自己使用权)
    public static readonly DependencyProperty MyNameProperty =
        InheritedDependencyOb.MyNameProperty.AddOwner(typeof(InheritDependencyOb),new FrameworkPropertyMetadata("",FrameworkPropertyMetadataOptions.Inherits));
}
```

```xml
<StackPanel>
    <local:InheritedDependencyOb MyName="Bob">
        <local:InheritDependencyOb x:Name="inheritDependencyOb"></local:InheritDependencyOb>
    </local:InheritedDependencyOb>
    <TextBlock Text="{Binding ElementName=inheritDependencyOb, Path=MyName}"></TextBlock>
</StackPanel>
```
输出Bob

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