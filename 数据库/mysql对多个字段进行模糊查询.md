### mysql对多个字段进行模糊查询

一般情况下，对多个字段进行模糊查询的sql写法是这样：

```
select * from Table1 where name like '%xxx%' or info like '%xxx%';
```

但是这样的查询语句执行起来效率是十分低下的，一两个字段还好，一旦需要模糊查询的字段比较多了，就会有问题，查询也会很慢。



更好的解决办法是：

使用mysql中的concat函数，将多个字段先拼接起来，然后再进行like的模糊匹配。如：

```sql
SELECT * FROM `magazine` WHERE CONCAT(`title`,`tag`,`description`) LIKE ‘%关键字%’
```





但是这样有个问题，如果这三个字段中有值为NULL，则返回的也是NULL，那么这一条记录可能就会被错过，怎么处理呢，我这边使用的是IFNULL进行判断，则sql改为：

 

```sql
<pre name="code" class="sql">SELECT * FROM `magazine` WHERE CONCAT(IFNULL(`title`,''),IFNULL(`tag`,''),IFNULL(`description`,'')) LIKE ‘%关键字%’

```

类似于如此则可以进行简单的多字段模糊搜索了。