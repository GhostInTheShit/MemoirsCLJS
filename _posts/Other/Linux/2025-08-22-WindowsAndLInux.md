---
categories: linux 
---

# Windows和linux双系统

有两个方法

- 按顺序直接安装
- 双硬盘双EFI

>[!TIP]
>这里不讲基础的分区操作.
>安装Windows可以使用微PE作为工具,好处是下载方便,优启通只有网盘不太方便.

## 顺序安装

先安装Windwos,再安装linux。

因为linux安装系统里面自带和windows系统共存的选项,但是反过来Windows里面没有双系统共存选项,所以后装Windwos会覆盖了Linux系统的启动项。导致linux无法打开.

## 双硬盘双EFI

如果你已经安装了linux,想再安装windows又不希望linux系统被破坏.

那么你把windows装到一个独立的硬盘,EFI也安装到那个硬盘,这样你的系统有两个独立的EFI启动区，你想要切换系统就要进入BIOS主动选择开机启动项，也可以调整默认开机的系统顺序。

- linux系统进入BIOS按F12或者DELETE(电脑不同按键也可能不同)
- Windwos10直接按住`Shift`键然后点重启，不要松开,会进入蓝色界面，选择疑难杂问->更多选择->UEFI固件设置,就会进入BIOS.


>[!TIP]
>没记错应该是要选择GPT系统安装,MBR不支持双系统,而且linux也默认GPT才行。