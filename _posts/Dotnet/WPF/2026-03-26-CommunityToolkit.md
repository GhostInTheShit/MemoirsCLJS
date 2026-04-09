# CommunityToolkit


ObservableObject已经实现了INotifyPropertyChanged.
```C#
class MyViewModel : ObservableObject
```


`[ObservableProperty]` 标记,自动生成属性和SetProperty.  
`[RelayCommand]` 自动生成RealyCommand
```c#
//这里会生成Title属性
[ObservableProperty]
private string _title;

//这里会生成SayHelloCommand
[RelayCommand]
private void SayHello()
{
    //
}
```


## Messengers

