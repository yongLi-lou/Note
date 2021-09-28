# 异常详细信息: System.InvalidCastException: 对象不能从 DBNull 转换为其他类型——的解决方法

异常详细信息: System.InvalidCastException: 对象不能从 DBNull 转换为其他类型。

 

当从数据库中统计字段值时，有时没有记录就会产生一个DBNull值，在.net应用程序中用null值判断就会出错。

此时要加以判断须要用 ：

```c#
object o =SqlHelper.ExecuteScalar (connectionString, CommandType.Text, selectString, parms);

 

 if (o!=System .DBNull .Value )

{
    val = Convert.ToInt32(o);

}
```

或者用：

```c#
if (!Convert.IsDBNull(o) )

{
    val = Convert.ToInt32(o);

}
```

