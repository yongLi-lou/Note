# C#dbcontext的entry.State 

Detached：对象存在，但未由对象服务跟踪。在创建实体之后、但将其添加到对象上下文之前，该实体处于此状态；
Unchanged：自对象加载到上下文中后，或自上次调用 System.Data.Objects.ObjectContext.SaveChanges() 方法后，此对象尚未经过修改；
Added：对象已添加到对象上下文，但尚未调用 System.Data.Objects.ObjectContext.SaveChanges() 方法；
Deleted：使用 System.Data.Objects.ObjectContext.DeleteObject(System.Object) 方法从对象上下文中删除了对象；
Modified：对象已更改，但尚未调用 System.Data.Objects.ObjectContext.SaveChanges() 方法。