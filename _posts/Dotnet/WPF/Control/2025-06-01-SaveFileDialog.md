---
categories: WPF
---

# 保存文件对话框

```C#
SaveFileDialog saveFileDialog = new SaveFileDialog();

//前面为描述后面为文件类型,这里为两组,开头和结尾不能有空格
saveFileDialog.Filter = "Text file|*.txt|C# file|*.cs";

saveFileDialog.InitialDirectory = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);

if(saveFileDialog.ShowDialog() == true)
    File.WriteAllText(saveFileDialog.FileName, txtEditor.Text);
```

>[!important]
>`AddExtension` - 默认为true，并确定SaveFileDialog是否应自动为文件名附加扩展名（如果用户省略）。扩展名将基于所选的过滤器，除非这是不可能的，在这种情况下，它将回退到DefaultExt属性（如果指定）。如果您希望应用程序能够保存没有文件扩展名的文件，则可能必须禁用此选项。  
`OverwritePrompt` - 默认为true，并确定如果用户输入的文件名将导致现有文件被覆盖，  SaveFileDialog - 是否应该要求确认。除非在非常特殊的情况下，您通常希望启用此选项。   
`Title` - 如果您想在对话框中使用自定义标题，则可以覆盖此属性。它默认为“另存为”或本地化的等效项，该属性对OpenFileDialog也有效。   
`ValidateNames` - 默认为true，除非它被禁用，否则它将确保用户在允许用户继续之前只输入有效的Windows文件名。
