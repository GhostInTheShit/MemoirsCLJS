# GridSplitter

表格分隔符

`ShowsPreview` 显示预览

## ResizeBehavior

* BasedOnAlignment 行为根据对齐方向,**默认值**
* CurrentAndNext 当前和下一个
* PreviousAndCurrent 上一个和当前
* PreviousAndNext 上一个和下一个

一般教程让`GridSplitter`在单独一个grid位置,那么它将设置左边空间和右边空间,行为模式应为`PreviousAndNext`.(一般教程会设置`Alignmeng`为`stretch`,其实重要的是Behavior的设置.)  
但是其实可以让它与上一个控件放置在一起,那么行为模式应该为`CurrentAndNext`

只能只能操控相邻的控件,无法控制相隔的控件(可以将相隔的嵌套为相邻的控件)