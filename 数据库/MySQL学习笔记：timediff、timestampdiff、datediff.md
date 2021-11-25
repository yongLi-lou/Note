# [MySQL学习笔记：timediff、timestampdiff、datediff](https://www.cnblogs.com/hider/p/9067010.html)

##  一、时间差函数：timestampdiff

　　**语法：**timestampdiff(interval, datetime1,datetime2)

　　**结果：**返回（时间2-时间1）的时间差，结果单位由interval参数给出。

- frac_second 毫秒（低版本不支持，用second，再除于1000）
- second 秒
- minute 分钟
- hour 小时
- day 天
- week 周
- month 月
- quarter 季度
- year 年

　　**注意：**MySQL 5.6之后才支持毫秒的记录和计算，如果是之前的版本，最好是在数据库除datetime类型之外的字段，再建立用于存储毫秒的int字段，然后自己进行转换计算。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# 所有格式
SELECT TIMESTAMPDIFF(FRAC_SECOND,'2012-10-01','2013-01-13'); # 暂不支持
SELECT TIMESTAMPDIFF(SECOND,'2012-10-01','2013-01-13'); # 8985600
SELECT TIMESTAMPDIFF(MINUTE,'2012-10-01','2013-01-13'); # 149760
SELECT TIMESTAMPDIFF(HOUR,'2012-10-01','2013-01-13'); # 2496
SELECT TIMESTAMPDIFF(DAY,'2012-10-01','2013-01-13'); # 104
SELECT TIMESTAMPDIFF(WEEK,'2012-10-01','2013-01-13'); # 14
SELECT TIMESTAMPDIFF(MONTH,'2012-10-01','2013-01-13'); # 3
SELECT TIMESTAMPDIFF(QUARTER,'2012-10-01','2013-01-13'); # 1
SELECT TIMESTAMPDIFF(YEAR,'2012-10-01','2013-01-13'); # 0
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

## 二、时间差函数：datediff

 　**语法：**传入两个日期参数，比较DAY天数，第一个参数减去第二个参数的天数值。

```
SELECT DATEDIFF('2013-01-13','2012-10-01'); # 104
```

 

## 三、时间差函数：timediff

　　**语法：**timediff(time1,time2)

　　**结果：**返回两个时间相减得到的差值，time1-time2

```
SELECT TIMEDIFF('2018-05-21 14:51:43','2018-05-19 12:54:43');
# 49:57:00
```

 

## 四、其他日期函数

- now()函数返回的是当前时间的年月日时分秒
- curdate()函数返回的是年月日信息
- curtime()函数返回的是当前时间的时分秒信息
- 对一个包含年月日时分秒日期格式化成年月日日期，可以使用DATE(time)函数

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# 其他日期函数
SELECT NOW(); # 2018-05-21 14:41:00
SELECT CURDATE(); # 2018-05-21
SELECT CURTIME(); # 14:41:38
SELECT DATE(NOW()); # 2018-05-21
SELECT SYSDATE(); # 2018-05-21 14:47:11
SELECT CURRENT_TIME(); # 14:51:30
SELECT CURRENT_TIMESTAMP; # 2018-05-21 14:51:37
SELECT CURRENT_TIMESTAMP(); # 2018-05-21 14:51:43
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**注意：**now()与sysdate()类似，只不过now()在执行开始时就获取，而sysdate()可以在函数执行时动态获取。