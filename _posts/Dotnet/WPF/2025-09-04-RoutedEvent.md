---
categories: WPF
---

# 路由事件

冒泡事件:由触发的控件向上转发依次到达根控件(Window)

隧道事件:由根控件(Window)向下转发,依次到达触发控件

一个事件一定会经历这两个过程,

例如: 一个Button被点击,那么事件的转发为

    隧道事件              冒泡事件
    Window->Grid->Button->Button->Grid->Window

事件处理器可以选择在这条路线上任意一个位置将其处理。


## 已处理

`Handled` 为 `ture` 实际上事件仍然会路由到下一个监听器，但不会处理。