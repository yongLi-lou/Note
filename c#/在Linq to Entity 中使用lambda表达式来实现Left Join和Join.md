# [在Linq to Entity 中使用lambda表达式来实现Left Join和Join](https://www.cnblogs.com/firstcsharp/p/6198407.html)

**1、读取用户和部门两个表的左连接：**

```
var sg = db.Users.GroupJoin(db.Departments, u => u.DepartmentId, d => d.DepartmentId, (u,d) => new { u, d }).Select(o=>o).ToList();
```

注意：上面将返回所用用户信息和对应的部门信息（即用户部门ID信息缺少，那么用户列表也会显示）

 

**2、读取指定返回列表字段的左连接信息：**

```
 var GJoinList = db.Sys_User.GroupJoin(db.Sys_Department, u => u.DepartmentId, d => d.DepartmentId, (u,d) => new { UserId=u.UserId, Account=u.Account, RealName=u.RealName, EnabledMark=u.EnabledMark, DeleteMark=u.DeleteMark,DepartmentName = d.FirstOrDefault(x=>x.DepartmentId==u.DepartmentId).FullName}).Select(o=>o);
```

 

**3、读取连接表：**

```
var sg = db.Users.Join(db.Departments, u => u.DepartmentId, d => d.DepartmentId, (u,d) => new { u, d }).Select(o=>o).ToList();
```

注意：这里将只显示用户里DepartmentId和部门表里DepartmentId相等的信息，如果用户没有部门ID则此条用户信息不会显示

```
 var data = db.CRM_OrderDetails.Join(db.CRM_Order, d => d.OrderId, o => o.OrderId, (d, o) => new { d, o }).Select(p => new ProductsListModel() {
                OrderId =p.o.OrderId,
                OrderCode=p.o.OrderCode,
                CustomerName=p.o.CustomerName,
                ProductName=p.d.ProductName,
                UnitId=p.d.UnitId,
                Qty=p.d.Qty,
                Price=p.d.Price,
                Amount=p.d.Amount,
                TaxAmount=p.d.TaxAmount,
                TaxCostAmount=p.d.TaxCostAmount,
                CreateTime=p.d.CreateTime,
                EndTime=p.d.EndTime,
                Description=p.d.Description
            }).Where(expression).OrderBy(orderbyExpression);
```



 