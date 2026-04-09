---
categories: Avalonia
---

# 行为

需要安装社区包`xaml.avalonia.behaviors`

可以单独挂载

## 使用


`SomethingBehavior:Behavior<>` <>里面是控件,你要附加到什么控件上,就写什么

`OnAttached()` 在附加时候执行

`OnDetaching()` 在分离时候执行

`AssociatedObject` 关联Object 指的是添加该Behavior的控件

>[!WARNING]
>keydown事件,只有获得Focus的控件才能触发。

C#

``` C#
public class SomethingBehavior : Behavior<button>
{

    protected override void OnAttached()
    {
        AssociatedObject.KeyDown+= OnKeyDown;
    }
    
    protected override void OnDetaching()
    {
        AssociatedObject.KeyDown -= OnKeyDown;
    }
    
    private void OnKeyDown(object? sender, KeyEventArgs e)
    {

        Console.WriteLine("Key pressed");
        e.Handled = true;
    }
}
```

Xaml

```Xaml
    xmlns:b="using:YourAPP.YourBehaviors">

    <Interaction.Behaviors>
        <b:SomethingBehavior></b:SomethingBehavior>
    </Interaction.Behaviors>

```