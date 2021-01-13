# Asp.net core 设置事务级别

```c#
public async Task Inside()
{
    using var scope = SqlCommonMethod.CreateTransactionScope();
    await SqlCommonMethod.SetIsolationLevel(Db, IsolationLevel.Serializable); // 开启高级别锁
    var countries = await Db.Countries.ToListAsync(); // counties 表被高级锁了
    await SqlCommonMethod.SetIsolationLevel(Db, IsolationLevel.ReadCommitted); // 设置回普通锁, 不然接下来都会一直是高级锁
    await Inside2();
    scope.Complete(); // 这里的 complete 结束并不会解锁, 会一直等到最外面的 scope 被释放, 符合逻辑
}

public async Task Inside2()
{
    using var scope = SqlCommonMethod.CreateTransactionScope();
    var countries = await Db.States.ToListAsync(); // // states 表被普通锁了
    scope.Complete();
}

#region Simple test 
[HttpPost("simple-test")]
public async Task<IActionResult> SimpleTest()
{
    using (var scope = SqlCommonMethod.CreateTransactionScope())
    {
        await Inside();
        scope.Complete(); // 这里依然不会解锁, 要等到 scope 释放
    } 
    // 直到 using 结束才会解锁 table 哦
    return Ok("ok");
}
```

总结 : 每次设置级别后最好是设置回去. 不然全场都会使用高级锁. 

 

ef core 有 unit of work 的概念，当我们 save change 时会自动使用 transaction 确保更新的一致性. 隔离级别是默认的 read committed 不允许脏读. 

但是呢, 有时候我们希望拥有更好的隔离级别, 比如 repeatable read, serializable 

那么就需要调用 database.beginTransaction 了. 

一旦需要自己控制 trans 麻烦就跟着来了。

比如在多个服务嵌套调用时, 如何共享 trans 呢 ? 

每个服务的 trans 级别也有可能是不同的.

如果我们单纯使用 beginTransaction 那么需要在每个服务写判断，是否有 current transaction 做出不同的处理.  

在早期, 我们会用 transaction scope 作为业务级别的事务. 

transaction scope 非常强大, 可以跨库, 分布式, 甚至可以链接 file system 

比如一个事务内做了数据库修改，也创建了 file， 如果事务最终失败，连 file 也可以 rollback 删除掉. 

但自从 ef 出现后, 这个就变得大材小用了些. ef 也不推荐我们使用了 refer https://docs.microsoft.com/zh-cn/ef/ef6/saving/transactions?redirectedfrom=MSDN

ef core 在 2.1 的时候支持了 transactionscope 但是不支持分布式, 好像是不兼容 linux 所以去掉了.

但是在我说的这种情况下，使用它依然会让代码变得更好.

调用起来是这样的

```c#
using (var scope = new TransactionScope(
                scopeOption: TransactionScopeOption.Required,
                transactionOptions: new TransactionOptions { IsolationLevel = System.Transactions.IsolationLevel.RepeatableRead },
                asyncFlowOption: TransactionScopeAsyncFlowOption.Enabled
            ))
            {
                try
                {
                 
                }
                catch (Exception ex)
                {
                    return BadRequest(ex.Message);
                }
                scope.Complete();
            }
```

