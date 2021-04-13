# [MySql中捕获异常的方法(转)](https://www.cnblogs.com/tonykan/archive/2012/12/06/2804259.html)

mySql中是否能有SQLserver的@@error变量呢，或者如c#中的try catch语法呢。

答案是肯定的，实例代码如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 DROP PROCEDURE IF EXISTS sp_call_jobs;
 2 CREATE PROCEDURE sp_call_jobs()
 3     NOT DETERMINISTIC
 4     SQL SECURITY DEFINER
 5     COMMENT ''
 6 BEGIN
 7 declare _row,_err,_count int default 0;
 8 DECLARE CONTINUE  HANDLER FOR SQLEXCEPTION,SQLWARNING,NOT FOUND set _err=1;
 9 while _row<3 DO
10   START TRANSACTION;
11      insert into t1(cond_val)values(null);
12   COMMIT;
13  if _err=1 then
14    set _count=_count+1;
15  end if;
16  set _row=_row+1;
17 end while;
18 select _count;
19 END;
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

语句：

DECLARE CONTINUE HANDLER FOR SQLEXCEPTION,SQLWARNING,NOT FOUND set _err=1;

作用是当遇到SQLEXCEPTION,SQLWARNING,NOT FOUND 错误时，设置_err=1并执行CONTINUE操作，即继续执行后面的语句。

这就与c＃中的try catch语法很像。

而且在执行可能出错的语句的时候我们用事务语句：START TRANSACTION; …… COMMIT; 可以保证完整性。