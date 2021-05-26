# Mysql权限问题: Access denied； you need (at least one of) the RELOAD privilege(s) for this operation

场景是使用某个mysql用户访问mysql的binlog, 出现这个问题的原因是当前用户没有reload权限导致的。

* 1、reload 是 administrative 级的权限，即 server administration；这类权限包括：

 ```sql
     CREATE USER, PROCESS, RELOAD, REPLICATION CLIENT, REPLICATION SLAVE, SHOW DATABASES, SHUTDOWN, SUPER
  
 ```

* 2、这类权限的授权不是针对某个数据库的，因此须使用on *.*来进行：
```sql
	 grant reload on *.* to 'test'@'localhost'
```

解决方法有三种：

一是在服务器上使用Navicat for MySQL登录数据库并且IP地址用localhost；

二是把DEFINER=`root`@`localhost`的localhost改为你的服务器IP；

三是在你的sql文件中删除DEFINER=`root`@`localhost`这个限制。
