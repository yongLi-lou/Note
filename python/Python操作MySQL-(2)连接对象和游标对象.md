# Python操作MySQL-(2)连接对象和游标对象

在pycharm中按照PyMySQL或者PyMySQL3即可，具体安装方法：
File->Settings->Project:->Project Interpreter选择右边的加号’+’查找即可。
然后就可以引入pymysql模块了。

连接对象
数据库连接对象，建立连接。
创建方法：pymysql.connect(参数)

参数名	类型	说明
host	字符串	MySQL服务器地址
port	数字	MySQL服务器端口号
user	字符串	用户名
passwd	字符串	密码
db	字符串	数据名称
charset	字符串	连接编码
connection连接对象支持的方法

方法名	说明
cursor()	使用该连接创建并返回游标
commit()	提交当前事务
rollback()	回滚当前事务
close()	关闭连接
接下来连接数据库测试。
首先在测试之前确认一下数据的编码格式，因为连接对象的最后一个参数需要编码格式。

如果需要修改数据库编码只需要：

```sql
ALTER DATABASE test CHARACTER SET = utf8;
```

最后是python代码连接测试。

```python
import pymysql

conn = pymysql.connect(
    host='127.0.0.1',
    port = 3306,
    user='root',
    passwd='',
    db='test',
    charset='utf8'
)
cur = conn.cursor()
print(conn)
print(cur)
#打印结果
<pymysql.connections.Connection object at 0x02581BD0>
<pymysql.cursors.Cursor object at 0x02241950> #一个游标对象
```

接下里看游标对象。

### **游标对象**

游标对象用于执行、查询和获取结果。
支持的方法

方法名	说明
execute(op[,args])	执行一个数据库的查询和命令
fetchone()	获取结果集的下一行
fetchmany(size)	获取结果集的下几行务
fetchall()	获取结果集中剩下的所有行
rowcount()	最近一次execute返回数据的行数或影响的行数
close()	关闭游标对象

其中execute方法是执行sql语句，并且将结果从数据库中获取到本地客户端。
关于fetch系列的使用见代码，使用的数据库是上一章初始化的：

```python
cur = conn.cursor() #获取游标对象

sql = 'SELECT * FROM tb1' #sql查询语句

cur.execute(sql) #执行sql语句

print(cur.rowcount)         #查看影响了多少行数据

res = cur.fetchone()        #获取一个结果
print(res)

res = cur.fetchmany(2)      #获取两个结果
print(res)

res = cur.fetchall()        #获取剩下所有的结果
print(res)

#打印结果
10      #第一次查询
(1, 'Jack') 
((2, 'Mark'), (3, 'Tom')) 
((4, 'A'), (5, 'B'), (6, 'C'), (7, 'D'), (8, 'E'), (9, 'F'), (10, 'G')) 
```

打印一下查看数据的内容：

```python
for row in cur.fetchall():
    print('id = %s, name = %s' % row)
#打印结果
id = 1, name = Jack
id = 2, name = Mark
id = 3, name = Tom
id = 4, name = A
id = 5, name = B
id = 6, name = C
id = 7, name = D
id = 8, name = E
id = 9, name = F
id = 10, name = G
```

事务
execute操作的集合称为事务。
事务：访问和更新数据库的一个程序执行单元。

原子性：事务中包括的所有操作只有都做和都不做连个选择。
一致性：事务必须使数据库从一致性状态变到另一个一致性状态。
隔离性：一个事务的执行不能被其他事务干扰。
持久性：一旦事务提交了，它对数据库的改变就是永久性的。
使用事务的方法：

关闭自动commit：设置conn.autocommit(False)
正常结束事务：conn.commit()
异常结束事务：conn.rollback()
注意不关闭自动更新事务，每一条sql语句都是会更新事务，假如有一条执行错误，那就GG了。默认关闭。

```python
cur = conn.cursor() #获取游标对象

sql_select = 'SELECT * FROM tb1' #sql查询语句
sql_updata = "UPDATE tb1 SET name='Rose' WHERE id=6"
sql_delete = 'DELETE FROM tb1 WHERE id<5'

cur.execute(sql_select)
print(cur.rowcount)

cur.execute(sql_updata)
print(cur.rowcount)

cur.execute(sql_delete)
print(cur.rowcount)

cur.close()
conn.close()

#打印结果
10
1
4
```

其实现在查看数据的表可以看出，表数据是没有变化的，正是因为没有提交事务所以不会更新数据表。
加上一句：conn.commit()再次运行。

显然数据已经更改。
其实上述代码有很大问题，因为操作数据库很可能发生异常，比如写错了字母等等。出异常之后所有事务需要回滚。
代码更正如下：

```python
cur = conn.cursor() #获取游标对象

sql_insert = "INSERT tb1(name) VALUES('Hi')" #sql查询语句
sql_updata = "UPDATE tb1 SET name='Rose' WHERE id=6"
sql_delete = 'DELETE FROM tb1 WHERE i<5' #注意故意写错了id

try:
    cur.execute(sql_insert)
    print(cur.rowcount)

    cur.execute(sql_updata)
    print(cur.rowcount)

    cur.execute(sql_delete)
    print(cur.rowcount)
except Exception as e:
    print(e)
    conn.rollback() #出错回滚

conn.commit()

cur.close()
conn.close()

#打印结果
1
1
(1054, "Unknown column 'i' in 'where clause'")
```

可以看出错误之后数据没有更新。