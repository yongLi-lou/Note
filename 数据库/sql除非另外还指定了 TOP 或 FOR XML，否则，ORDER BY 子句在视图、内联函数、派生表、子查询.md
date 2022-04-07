# [sql:除非另外还指定了 TOP 或 FOR XML，否则，ORDER BY 子句在视图、内联函数、派生表、子查询](https://www.cnblogs.com/zhangqs008/p/3655631.html)

执行sql语句:

select * from (

select * from tab where ID>20 order by userID desc

) as a order by date desc



逻辑上看着挺对 但是报错:

**除非另外还指定了 TOP 或 FOR XML，否则，ORDER BY 子句在视图、内联函数、派生表、子查询和公用表表达式中无效。**

**
**

只要我们在嵌套子查询视图里面加入: top 100 percent 即可

select * from (

select **top 100 percent** * from tab where ID>20 order by userID desc

) as a order by date desc







默认情况下，如果在子查询，函数，视图中尝试去使用ORDER BY，

> ```
> CREATE VIEW dbo.VSortedOrders
> AS
> 
> SELECT orderid, customerid
> FROM dbo.Orders
> ORDER BY orderid
> GO
> ```

 

那么可能会遇到下面的错误

> ```
> 消息 1033，级别 15，状态 1，第 4 行
> 
> 除非另外还指定了 TOP 或 FOR XML，否则，ORDER BY 子句在视图、内联函数、派生表、子查询和公用表表达式中无效。
> ```

```
原因就是针对一个表的SELECT其实并不是返回一个表，而是一个游标。
 
如果一定要用怎么办呢？答案就是配合TOP 100 PERCENT
SELECT     TOP (100) PERCENT orderid, customerid
FROM         dbo.Orders
ORDER BY orderid, customerid DESC
```