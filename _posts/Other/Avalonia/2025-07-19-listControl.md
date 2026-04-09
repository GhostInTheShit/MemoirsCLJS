---
categories: Avalonia
---

# 数据控件

数据列表

listbox

itemControl

## Property

`ItemsSource` 数据来源 需要Binding

`ListBoxItem` 里面单独的item控件 是个透明的框(可以和鼠标交互,修改背景颜色没用) 

## Template

``` Xaml

<ItemsControl>

    <!--这是设置布局-->
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <StackPanel Orientation="Horizontal"></StackPanel>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>

    <!--这是设置数据模板-->
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Vertical" Margin="5,0,5,0">
                <TextBlock Text="{Binding Name}" HorizontalAlignment="Center">
                <TextBlock>
            </StackPanel>
        </DataTemplate>
    </ItemsControl.ItemTemplate>

    <!--这是设置样式-->
    <ItemsControl.ItemContainerStyle>
    </ItemsControl.ItemContainerStyle>

</ItemsControl>
```

用法

``` xaml
<Control>
    <!--这里应该是new了一个ItemTemplate覆盖了原来的模板-->
    <Control.ItemTemplate>
        <!--数据模板 这里面Bindind的已经是集合里面的单个元素,类似foreach-->
        <DateTemplate>
        </DateTemplate>
    </Control.ItemTemplate>
</Control>
```
    


