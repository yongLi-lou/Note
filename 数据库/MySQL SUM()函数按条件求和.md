# MySQL SUM()函数按条件求和

- 一般求和

```
select sum(money) from user group by id;1
```

- 按条件求和

```
select sum(if(type=1,money,0)) from user group by id;
```

 在mysql中if()函数的用法类似于java中的三目表达式，其用处也比较多，具体语法如下：

IF(expr1,expr2,expr3)，如果expr1的值为true，则返回expr2的值，如果expr1的值为false，

则返回expr3的值。

<font color='red'> **所以求和的时候expr3为0**</font>

