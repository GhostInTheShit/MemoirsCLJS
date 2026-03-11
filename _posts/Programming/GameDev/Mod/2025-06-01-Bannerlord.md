---
categories: something
---

# 骑砍2

.Net语言制作mod

## 工具

DnSpy 反编译工具

Harmony : 对原来的方法进行修改或补丁

应用程序总是会用系统目录的一些库文件，而程序加载的时候，
是先从自己的文件夹内搜索库文件，如果找不到，再去系统目录查找。

**网络中很多教程代码已经不可用，进游戏会报错,要找新一点的**

## 结构

Game/Moudles/MODNAME/bin/SubModule.xml Mod配置文件,版本和依赖,

Game/Moudles/MODNAME/bin/Win64_Shipping_Client/MOD.dll Mod文件

**建议使用VS的骑砍模板** 可以解决配置文件和开发中的dll依赖

\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
UnityModManager,BepInEx 没用过
