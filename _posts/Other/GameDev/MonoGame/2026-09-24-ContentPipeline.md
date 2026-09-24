---
categories: Monogame
---

# Content Pipeline

MGCB 会将Asset转化为xnb.

直接点击项目中的`Content.mgcb`就可以打开内容编辑器

内置Asset

*   Effect（.fx）：一个着色器文件，通过控制GPU上的图形渲染方式来创建自定义视觉效果。
*   SpriteFont 描述（.spritefont）：定义文本在游戏中如何显示的配置文件，包括字符集和字体属性。
*   SpriteEffect（.fx）：专为2D精灵设计的着色器，用于创建特殊视觉效果。
*   XML Content（.xml）：用于存储游戏信息（如关卡、对话或配置设置）的结构化数据文件。
*   LocalizedSpriteFont Description（.spritefont）：用于创建支持多种语言字体的配置文件。

`Content.Load<T>("Path")` 加载

`Content.UnLoad()`卸载所有