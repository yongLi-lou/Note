# [FROM_UNIXTIME 格式化MYSQL时间戳函数](https://www.cnblogs.com/xieqian111/p/5735952.html)

函数：FROM_UNIXTIME
作用：将MYSQL中以INT(11)存储的时间以"YYYY-MM-DD"格式来显示。
语法：**FROM_UNIXTIME(unix_timestamp,format)**

返回表示 Unix 时间标记的一个字符串，根据format字符串格式化。format可以包含与DATE_FORMAT()函数列出的条目同样的修饰符。

根据format字符串格式化date值。
下列修饰符可以被用在format字符串中：

%M 月名字(January……December)
%W 星期名字(Sunday……Saturday)
%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。）
%Y 年, 数字, 4 位
%y 年, 数字, 2 位
%a 缩写的星期名字(Sun……Sat)
%d 月份中的天数, 数字(00……31)
%e 月份中的天数, 数字(0……31)
%m 月, 数字(01……12)
%c 月, 数字(1……12)
%b 缩写的月份名字(Jan……Dec)
%j 一年中的天数(001……366)
%H 小时(00……23)
%k 小时(0……23)
%h 小时(01……12)
%I 小时(01……12)
%l 小时(1……12)
%i 分钟, 数字(00……59)
%r 时间,12 小时(hh:mm:ss [AP]M)
%T 时间,24 小时(hh:mm:ss)
%S 秒(00……59)
%s 秒(00……59)
%p AM或PM
%w 一个星期中的天数(0=Sunday ……6=Saturday ）
%U 星期(0……52), 这里星期天是星期的第一天
%u 星期(0……52), 这里星期一是星期的第一天
%% 一个文字“%”。

例子：

```
SELECT FROM_UNIXTIME(1234567890, '%Y-%m-%d %H:%i:%S')
```

**SELECT \*,FROM_UNIXTIME(created, '%Y-%m-%d') as riqi FROM `rc_ms_users`**