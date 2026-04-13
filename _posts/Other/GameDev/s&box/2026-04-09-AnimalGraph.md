---
categories : s&box
---

# AnimalGraph

s&box 用来设计动作的工具.

在SkinnedModelRenderer之下.

## Animation Clip

动画切片

可设置是否循环.

## Tags

动画帧事件

使用一下代码接收事件,并不能单独接收某个名字的事件,而是先接收然后筛选名字.  
`ModelRenderer.OnAnimationEvent += HandleAnimEvent;`

## Paras

参数

## Status Machine

状态机

动画的逻辑.

### Condition

转化条件.

## blend

### 1D Blendspace 

一维混合,一般用来混合相似的动作.

`Sync Cycles` 同步循环,将动作长度调制相同,防止动作抽搐.

### 

二维混合 用的多