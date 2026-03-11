---
categories: Sqlserver
---

# 内连接

`select * from A,B`

得到的结果是叉乘(例A有6行，B有8行，那么答案为48行,A的数据和B的每行组合一次.最后的结果就是A的每行和B的每行组合了一次)

`select * from A Join B on 1=1`

链接B表，on为条件，这里的结果就是叉乘.和上面一样

| `select * from emp ,dept where emp.deptno=dept.deptno`
| `select * from emp join dept on emp.deptno=dept.deptno`
| 以上两条效果一样

可以多个表链接 join on 可以使用多次

where不能写在join on 前面，只能先链接再筛选

**当结果又有聚合查询又有普通查询，那么需要把聚合的表再次与其他表链接** :

    select e.deptno,e.[avg.sal],grade from
        (select deptno , avg(sal) 'avg.sal'  from emp
            join salgrade
            on emp.sal between salgrade.losal and salgrade.hisal
            group by deptno) "e"
        join salgrade
        on e.[avg.sal] between salgrade.losal and salgrade.hisal

当一个表叉乘自己的时候 :

    --A表joinB表等于B表joinA表。即使AB表可以对换，但是条件并不会重复。
    --可能会觉得 on  e.mgr=m.empno 等价于 on m.mgr=e.empno ,但是始终只有其中之一会实现而不会同时实现。所以条件没有交叉重复。
    select distinct e.mgr,m.empno, m.ename  from emp "e"
        join emp "m"
        --这里的on条件并没有错.
        on  e.mgr=m.empno

------------------------------------------------------------------------

如果多个表有同名字段,查询同名字段必须指明哪个表的哪个字段，可以用\*全部输出

（not in） 与 null 会导致全空
