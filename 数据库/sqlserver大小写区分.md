# sqlserver大小写区分

sqlserver默认不区分大小写

**第一种:**

ALTER TABLE tb  --（指定某表的某列）



ALTER COLUMN colname nvarchar(100) COLLATE Chinese_PRC_CI_AS      --不区分**[大小写](https://outofmemory.cn/tag/17955.html)**

ALTER TABLE tb  --（指定某表的某列）

ALTER COLUMN colname nvarchar(100) COLLATE Chinese_PRC_CS_AS     --区分大小写

alter database 数据库 COLLATE Chinese_PRC_CS_AS --（指定整个数据库）

**第二种:**

--创建如下用户自定义函数(UDF)



CREATE FUNCTION StrComp(@Str1 VARCHAR(50),@Str2 VARCHAR(50))

--ALTER FUNCTION StrComp(@Str1 VARCHAR(50),@Str2 VARCHAR(50))

RETURNS INTEGER

AS

BEGIN

 DECLARE @i INTEGER

 --DECLARE @Str1 VARCHAR(50)

 --DECLARE @Str2 VARCHAR(50)

 DECLARE @y INT

 --SET @Str1='a'

 --SET @Str2='A'

 SET @i=0

 --SELECT ASCII(SUBSTRING(@Str1,@i+1,1))

 SET @y=1

 DECLARE @iLen INT

 SET @iLen = LEN(LTRIM(RTRIM(@Str1)))

 IF LEN(LTRIM(RTRIM(@Str1))) < LEN(LTRIM(RTRIM(@Str2))) --THEN

   SET @iLen = LEN(LTRIM(RTRIM(@Str2)))

 WHILE (@i < @iLen)

  BEGIN

   IF (ASCII(SUBSTRING(@Str1,@i+1,1))=ASCII(SUBSTRING(@Str2,@i+1,1))) --THEN

​     SET @i = @i +1

   ELSE

​     BEGIN

​      SET @y=0

​      BREAK

​     END

   END

   RETURN @y

END

测试:

select *



from Table1

Where dbo.StrComp(Field1,'aAbB') =1

**第三种:**

SQL Server 数据库中的文本信息可以用大写字母、小写字母或二者的组合进行存储。例如，姓氏可以"SMITH"、"Smith"或"smith"等形式出现。

数据库是否区分大小写取决于 SQL Server 的安装方式。**[如果](https://outofmemory.cn/tag/169259.html)**数据库区分大小写，当搜索文本数据时，必须用正确的大小写字母组合构造搜索条件。例如，如果搜索名字"Smith"，则不能使用搜索条件"=smith"或"=SMITH"。

另外，如果服务器被安装成区分大小写，则必须用正确的大小写字母组合提供数据库、所有者、表和列的名称。如果提供的名称大小写不匹配，则 SQL Server 返回错误，报告"无效的对象名"。

当使用关系图窗格和网格窗格创建查询时，查询设计器始终正确地反映出服务器是否区分大小写。但是，如果在 SQL 窗格中输入查询，则必须注意使名称与服务器解释名称的方式相匹配。

如果服务器是用不区分大小写的选项安装的，则

提示  若要确定服务器是否区分大小写，请执行存储过程 sp_server_info，然后检查第 18 行的内容。如果服务器是用不区分大小写的设置安装的，则 sort_order 选项将设置为"不区分大小写"。可以从查询分析器运行存储过程。

**第四种：**

select * from servers where convert(varbinary, name)=convert(varbinary, N'RoCKEY')

**第五种：**

ascii('a')再配合Substring()一起用



假设要比较的两个**[日期](https://outofmemory.cn/tag/17065.html)** 分别为 rq1，rq2



则比较：

  select  case when rq1-rq2>=0 then  '该日期rq2小于等于日期rq1' else '该日期rq2大于于等于日期rq1'  end  

与系统日期比较：

select  case when GETDATE ()-rq1>=0 then  '该日期rq1小于等于当前系统日期' else '该日期rq1大于等于当前系统日期'  end