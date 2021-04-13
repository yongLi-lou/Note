# [Mysql 存储过程基本语法](https://www.cnblogs.com/hanlong/p/5714734.html)

**delimiter //
一般情况下MYSQL以；结尾表示确认输入并执行语句，但在存储过程中；不是表示结束，因此可以用该命令将；号改为//表示确认输入并执行。**

**一.创建存储过程
**

1.基本语法：

create procedure sp_name()
begin
.........
end

2.参数传递

**二.调用存储过程
**

1.基本语法：call sp_name()
注意：存储过程名称后面必须加括号，哪怕该存储过程没有参数传递

**三.删除存储过程
**

1.基本语法：
drop procedure sp_name//
2.注意事项
(1)不能在一个存储过程中删除另一个存储过程，只能调用另一个存储过程

**四.区块，条件，循环
**

1.区块定义，常用
begin
......
end;
也可以给区块起别名，如：
lable:begin
...........
end lable;
可以用leave lable;跳出区块，执行区块以后的代码
2.条件语句

![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)if 条件 then
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) statement
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) else
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) statement
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) end if;


3.循环语句
(1).while循环

![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)[label:] WHILE expression DO
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) statements
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) END WHILE [label] ;
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)

 

(2).loop循环

![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)[label:] LOOP
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) statements
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) END LOOP [label];

(3).repeat until循环

![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)[label:] REPEAT
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) statements
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) UNTIL expression
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif)
![Mysql <wbr>存储过程基本语法](http://images.csdn.net/syntaxhighlighting/OutliningIndicators/None.gif) END REPEAT [label] ;

***\*五.其他常用命令
\****

1.show procedure status
显示数据库中所有存储的存储过程基本信息，包括所属数据库，存储过程名称，创建时间等
2.show create procedure sp_name
显示某一个存储过程的详细信息

下面看一个例子

**一、MySQL 创建存储过程**

“pr_add” 是个简单的 MySQL 存储过程，这个MySQL 存储过程有两个 int 类型的输入参数 “a”、“b”，返回这两个参数的和

delimiter //  -- 改变分割符

drop procedure if exists pr_add// -- 若之前创建有这个存储过程则删除

计算两个数之和

1. create procedure pr_add (a int,b int)
2. begin
3. declare c int;
4. if a is null then
5. set a = 0;
6. end if;
7. if b is null then
8. set b = 0;
9. end if;
10. set c = a + b;
11. select c as sum;
12. end
13. //

**二、调用 MySQL 存储过程**

1. call pr_add(10, 20);

执行 MySQL 存储过程，存储过程参数为 MySQL 用户变量。

1. set @a = 10;
2. set @b = 20;
3. call pr_add(@a, @b);

**三、MySQL 存储过程特点**

创建 MySQL 存储过程的简单语法为：

1. create procedure 存储过程名字()
2. (
3. [in|out|inout] 参数 datatype
4. )
5. begin
6. MySQL 语句;
7. end;

MySQL 存储过程参数如果不显式指定“in”、“out”、“inout”，则默认为“in”。习惯上，对于是“in” 的参数，我们都不会显式指定。

\1. MySQL 存储过程名字后面的“()”是必须的，即使没有一个参数，也需要“()”

\2. MySQL 存储过程参数，不能在参数名称前加“@”，如：“@a int”。下面的创建存储过程语法在 MySQL 中是错误的（在 SQL Server 中是正确的）。 MySQL 存储过程中的变量，不需要在变量名字前加“@”，虽然 MySQL 客户端用户变量要加个“@”。

1. create procedure pr_add
2. (
3. @a int, -- 错误
4. b int -- 正确
5. )

\3. MySQL 存储过程的参数不能指定默认值。

\4. MySQL 存储过程不需要在 procedure body 前面加 “as”。而 SQL Server 存储过程必须加 “as” 关键字。

1. create procedure pr_add
2. (
3. a int,
4. b int
5. )
6. as -- 错误，MySQL 不需要 “as”
7. begin
8. mysql statement ...;
9. end;

\5. 如果 MySQL 存储过程中包含多条 MySQL 语句，则需要 begin end 关键字。

1. create procedure pr_add
2. (
3. a int,
4. b int
5. )
6. begin
7. mysql statement 1 ...;
8. mysql statement 2 ...;
9. end;

\6. MySQL 存储过程中的每条语句的末尾，都要加上分号 “;”

1. ...
2. declare c int;
3. if a is null then
4. set a = 0;
5. end if;
6. ...
7. end;

\7. MySQL 存储过程中的注释。

1. declare c int; -- 这是单行 MySQL 注释 （注意 -- 后至少要有一个空格）
2. if a is null then # 这也是个单行 MySQL 注释
3. set a = 0;
4. end if;
5. ...
6. end;

\8. 不能在 MySQL 存储过程中使用 “return” 关键字。

1. set c = a + b;
2. select c as sum;
3. end;

\9. 调用 MySQL 存储过程时候，需要在过程名字后面加“()”，即使没有一个参数，也需要“()”

1. call pr_no_param();

**10. 因为 MySQL 存储过程参数没有默认值，所以在调用 MySQL 存储过程时候，不能省略参数。可以用 null 来替代。**

```sql
DROP PROCEDURE IF EXISTS Proc_Change;
CREATE PROCEDURE Proc_Change (id CHAR(4),toid CHAR(4),money INT,OUT msg VARCHAR(20))
BEGIN
DECLARE _error INT DEFAULT 0;
DECLARE CONTINUE  HANDLER FOR SQLEXCEPTION,SQLWARNING set _error = 1;
UPDATE bank SET bank.balance=bank.balance-money WHERE bank.cId=id;
UPDATE bank SET bank.balance=bank.balance+money WHERE bank.cId=toid;
IF _error = 1 THEN
SET msg='转账失败';
ROLLBACK;
ELSE
set msg='转账成功';
COMMIT;
END IF;
END

```



