# EF Core事务提交，分布式事务

**EF Core的SaveChanges方法本身就是事务**

 

**但是如果多个SaveChanges方法提交，则需用IDbContextTransaction**

```c#
using (EFCoreContext context = new EFCoreContext())
{
    IDbContextTransaction tran = null;
    try
    {
        tran = context.Database.BeginTransaction();
        context.UserInfo.Add(new UserInfo()
        {
            Name = "haha11",
            Age = 19
        });
        context.SaveChanges();
 
        context.UserInfo.Add(new UserInfo()
        {
            Name = "haha22",
            Age = 19
        });
        context.SaveChanges();
 
        tran.Commit();
    }
    catch (Exception ex)
    {
        if (tran != null)
            tran.Rollback();
 
        Console.WriteLine(ex.Message);
    }
    finally
    {
        tran.Dispose();
    }
 
}
```

 

**如果是多个Context，即分布式事务，则需用TransactionScope**

```c#
using (EFCoreMigrationContext context1 = new EFCoreMigrationContext())
using (EFCoreMigrationContext context2 = new EFCoreMigrationContext())
{
    using (TransactionScope transactionScope = new TransactionScope())
    {
        try
        {
            context1.UserInfo.Add(new UserInfo()
            {
                Name = "hehe11",
                Age = 20
            });
            context1.SaveChanges();
 
            context2.UserInfo.Add(new UserInfo()
            {
                Name = "hehe22",
                Age = 20
            });
            context2.SaveChanges();
 
            transactionScope.Complete();
 
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
}
```

 