---
categories: ASP.net
---

# Repository

仓储模式

之前`Services`->`Dbcontext`

现在`Services`->`Repository`->`Dbcontext`

中间加一次仓储层,这样`services`就不直接调用`Dbcontext`,中间有一层接口,可以更改调用方法,比如从`EFCore`换成`Azure`.要不然`Service`中直接写死了`EFCore`.

具体实现

添加两个项目  
`RepositoryContracts` 接口,定义操作数据库的方法

`Repositories` 具体实现,直接操作数据(不是DTO),不进行判断,判断交给业务层.

## 好处

加入了中间层 松耦合

方便迁移到另一套数据库模式

方便单元测试,不需要Mock Dbcontext

