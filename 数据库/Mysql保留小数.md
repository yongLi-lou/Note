# Mysql保留小数

1. round(x,d) :用于数据的四舍五入,round(x)  ,其实就是round(x,0),也就是默认d为0；

这里有个值得注意的地方是，d可以是负数，这时是指定小数点左边的d位整数位为0,同时小数位均为0；

SELECT ROUND(100.3465,2),ROUND(100,2),ROUND(0.6,2),ROUND(114.6,-1);

结果分别：100.35,100，0.6,110

2. TRUNCATE(x,d)：函数返回被舍去至小数点后d位的数字x。若d的值为0，则结果不带有小数点或不带有小数部分。若d设为负数，则截去（归零）x小数点左起第d位开始后面所有低位的值。

   SELECT TRUNCATE(100.3465,2),TRUNCATE(100,2),TRUNCATE(0.6,2),TRUNCATE(114.6,-1);

   结果分别：100.34,100，0.6,110

3. FORMAT（X,D）：强制保留D位小数，整数部分超过三位的时候以逗号分割，并且返回的结果是string类型的

   　SELECT FORMAT(100.3465,2),FORMAT(100,2),FORMAT(,100.6,2);

   结果分别：100.35,100.00，100.60

4. convert（value，type）;类型转换，相当于截取``

   type:

   - 二进制，同带binary前缀的效果 : BINARY   
   - 字符型，可带参数 : CHAR()   
   - 日期 : DATE   
   - 时间: TIME   
   - 日期时间型 : DATETIME   
   - 浮点数 : DECIMAL    
   - 整数 : SIGNED   
   - 无符号整数 : UNSIGNED 

   SELECT CONVERT(100.3465,DECIMAL(10,2)), CONVERT(100,DECIMAL(10,2)),CONVERT(100.4,DECIMAL(10,2));

   结果分别：100.35,100，100.4