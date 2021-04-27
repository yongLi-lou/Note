### mysql对多个字段进行模糊查询

一般情况下，对多个字段进行模糊查询的sql写法是这样：

```
select * from Table1 where name like '%xxx%' or info like '%xxx%';
```

但是这样的查询语句执行起来效率是十分低下的，一两个字段还好，一旦需要模糊查询的字段比较多了，就会有问题，查询也会很慢。



更好的解决办法是：

使用mysql中的concat函数，将多个字段先拼接起来，然后再进行like的模糊匹配。如：

```
select * from Table1 where concat('name','info') like '%xxx%';
```