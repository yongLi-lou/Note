# mysql怎么创建和调用out参数的存储过程

```
CREATE PROCEDURE sp_add(a int, b int,out c int)
begin

 set c=a+ b;

end;
调用过程：
call sp_add (1,2,@a);
select @a;
```