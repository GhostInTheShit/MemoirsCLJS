---
categories: WPF
---

# DockPanel

DockPanel 可以将子控件停靠在顶部、底部、左侧或右侧。默认情况下，最后一个子控件会填充剩余空间。

```xml
<DockPanel>
    <!-- DockPanel.Dock 是由 DockPanel 提供的附加属性 -->
    <Button DockPanel.Dock="Left">Left</Button>
    <Button DockPanel.Dock="Top">Top</Button>
    <Button DockPanel.Dock="Right">Right</Button>
    <Button DockPanel.Dock="Bottom">Bottom</Button>
    <Button>Center</Button>
</DockPanel>
```

设置 `<DockPanel LastChildFill="False">` 可以禁用最后一个子控件填充剩余空间的行为。

> [!warning]
> 如果只有一个子控件，它会填满 DockPanel。
