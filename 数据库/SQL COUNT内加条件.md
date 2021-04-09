# SQL COUNT内加条件

后面要加or null

```sql
SELECT   COUNT(pm.State=5 or NULL) as 完成数量 ,COUNT(pm.State!=5) 未完成数量,u.ID AS 用户ID,u.`Name` AS 用户姓名 FROM Patrol_Mission pm,SysUser u WHERE pm.UserID=u.ID GROUP BY u.ID,u.`Name`
```

如count(release_year = '2006' or NULL) 这部分 为什么要加上or NULL 直接count(release_year='2006')有什么问题吗？不就是要找release_year = '2006'的数据吗，为什么要计算NULL的数据

答案：

因为 当 release_year不是 2006时 ，release_year='2006' 结果false 不是 NULL，

Count在 值是NULL是 不统计数， （count('任意内容')都会统计出所有记录数，因为count只有在遇见null时不计数，即count(null)==0，因此前者单引号内不管输入什么值都会统计出所有记录数）至于加上or NULL ， 很像其他编程里的or运算符，第一个表达式是true就是不执行or后面的表达式，第一个表达式是false 执行or后面的表达式 。当release_year不为2006时release_year = '2006' or NULL 的结果是NULL，Count才不会统计上这条记录数

## **count函数只有在值为null的时候才不会统计，(count(‘任意内容’)都会统计出所有记录数。**