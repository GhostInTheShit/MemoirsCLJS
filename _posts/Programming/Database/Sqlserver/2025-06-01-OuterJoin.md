---
categories: Sqlserver
---

# 外连接

外连接

> 左连接 `left join`
> 相比内连接把左边行不成立的至少输出一行，以左表为主要观察对象查看有和没有的情况.
>
> 右连接 `right join`

完全连接

> `full join` 左右表不成立的都输出.

交叉连接

> `cross join` 就是笛卡儿积，等价于 `join xxx on 1=1` 或者 `from A,B`

自连接

> 自己和自己连接

联合

> `union` 表和表纵向的连接(行数的增加)
