---
categories: WPF
---

# MessageBox

消息框

```C#
MessageBox.Show(
//文本
"Hello, world?", 
//标题
"My App", 
//信息框按钮
MessageBoxButton.YesNo, 
//信息框图片
MessageBoxImage.Question, 
//默认被选择的按钮
MessageBoxResult.No);
```

返回值的应用

```C#
//有返回值
MessageBoxResult result = MessageBox.Show(
//第一个是content
"Would you like to greet the world with a \"Hello, world\"?",
//标题 
"My App",
//消息按钮 
MessageBoxButton.YesNoCancel);

switch(result)
{
    case MessageBoxResult.Yes:
        MessageBox.Show("Hello to you too!", "My App");
        break;
    case MessageBoxResult.No:
        MessageBox.Show("Oh well, too bad!", "My App");
        break;
    case MessageBoxResult.Cancel:
        MessageBox.Show("Nevermind then...", "My App");
        break;
}
```
