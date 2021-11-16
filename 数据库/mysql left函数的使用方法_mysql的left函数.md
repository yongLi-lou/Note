# mysql left函数的使用方法_mysql的left函数

1、LEFT()函数是一个字符串函数，它返回具有指定长度的字符串的左边部分。

LEFT(Str,length);

接收两个参数：

str：一个字符串；

length：想要截取的长度，是一个正整数；

2、示例：

SELECT LEFT('2019-01-30',0);

SELECT LEFT('',3);

结果为空；

SELECT LEFT('2019-01-30',NULL);

SELECT LEFT(NULL,3);

结果为NULL;

3、REVERSE(Str)：翻转，这个函数可以将字符串翻转；

SELECT REVERSE(LEFT('2019-01-30',4));

取前4位并翻转。

4、日期函数

4.1、SELECT DATE_FORMAT(CURDATE(),'%Y%m');

结果为当前年和月：201901

4.2、SELECT CURRENT_TIMESTAMP;

结果为当前日期及时间：2019-01-29 23:59:31

4.3、SELECT CURRENT_TIME;

结果为当前时间：23:59:31

4.4、SELECT CURRENT_DATE;

结果为当前日期：2019-01-29
