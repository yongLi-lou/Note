# TSQL存储过程返回表信息

### 第一步创建存储过程

```sql
if EXISTS(SELECT * FROM sysobjects where name='DataTest')
DROP PROC DataTest
GO

CREATE PROC DataTest
as 
BEGIN



END
```



### 第二步创建临时表

```sql
create table #temTable(
  Lagend varchar(200),
  Year int,
  Jan DECIMAL,

Feb DECIMAL,
Mar DECIMAL,
Apr DECIMAL,
May DECIMAL,
Jun DECIMAL,
Jul DECIMAL,
Aug DECIMAL,
Sep DECIMAL,
Oct DECIMAL,
Nov DECIMAL,
Dec DECIMAL,
)
```

### 第三步查询数据

```sql
insert into #temTable VALUES ('测试',2022,1,2,3,4,5,6,7,8,9,10,11,12)
insert into #temTable VALUES ('测试2',2022,100,2,3,4,5,6,7,8,9,10,11,12)

select  *
from #temTable  
```

### 第四步删除临时表

```sql
drop table #temTable
```

### 完整

```sql
if EXISTS(SELECT * FROM sysobjects where name='DataTest')
DROP PROC DataTest
GO

CREATE PROC DataTest
as 
BEGIN

create table #temTable(
  Lagend varchar(200),
  Year int,
  Jan DECIMAL,

Feb DECIMAL,
Mar DECIMAL,
Apr DECIMAL,
May DECIMAL,
Jun DECIMAL,
Jul DECIMAL,
Aug DECIMAL,
Sep DECIMAL,
Oct DECIMAL,
Nov DECIMAL,
Dec DECIMAL,
)

insert into #temTable VALUES ('测试',2022,1,2,3,4,5,6,7,8,9,10,11,12)
insert into #temTable VALUES ('测试2',2022,100,2,3,4,5,6,7,8,9,10,11,12)

select  *
from #temTable  

drop table #temTable

END

```

