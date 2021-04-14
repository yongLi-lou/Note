# [Err] 1055 - Expression #1

异常：在执行一段SQL时出现以下问题

[Err] 1055 - Expression #1 of ORDER BY clause is not in GROUP BY clause and contains nonaggregated column ‘information_schema.PROFILING.SEQ’ which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

分析：

根据异常信息分析能发现，这个问题主要是由于sql_mode引起，所以先查看sql_mode后修改即可，主要是去除only_full_group_by

解决方案：
方案一：只修改当前数据库的SQL_MODE

-- 查看SQL_MODE
SELECT @@sql_mode;

-- 修改SQL_MODE
SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));

方案二：修改配置文件my.ini

-- 查看数据库的存储地址
SELECT @@datadir

-- D:\MySQL\mysql-5.7.17-winx64\data\


打开对应的地址，找到my.ini，添加

[mysql] 
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
