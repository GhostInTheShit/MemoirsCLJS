---
categories: WPF
---

# 文件对话框 OpenFileDialog

```C#
//创建一个文件对话框
OpenFileDialog openFileDialog = new OpenFileDialog();

//是否可以多选
openFileDialog.Multiselect = true;
//文件的类型
openFileDialog.Filter = "Text files (*.txt)|*.txt|All files (*.*)|*.*";
//默认路径，使用了环境类
openFileDialog.InitialDirectory = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
	
//openFileDialog.ShowDialog()是可空布尔类型,当用户点击打开时候会为true
if(openFileDialog.ShowDialog() == true)
{
    //openFileDialog.FileName 如果是单选,文件名
    //openFileDialog.FileNames 多选的文件名,是一个集合
}
```
>[!Warning]
>在处理文件和IO任务时候,你应该要注意引发的异常问题.
