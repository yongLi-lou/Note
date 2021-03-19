# MySql支持Emoji表情

### **1.原因**：

UTF-8编码有可能是两个、三个、四个字节。Emoji表情是4个字节，而Mysql的utf8编码最多3个字节，所以数据插不进去。

### **2.解决方案**：

将Mysql的编码从utf8转换成utf8mb4。详细说明如下：

首先停止MySQL Server服务，修改mysql配置文件 my.cnf（其他系统）或者mysql.ini（windows系统）

[client]

default-character-set = utf8mb4

[mysql]

default-character-set = utf8mb4

[mysqld]

character-set-client-handshake = FALSE

character-set-server = utf8mb4

collation-server = utf8mb4_unicode_ci

init_connect='SET NAMES utf8mb4'



修改完成，重启 MySQL 服务:

#### 用MYSQL X.X Command Line Client修改数据库、表字符集

检查字符集修改结果，打开MYSQL X.X Command Line Client

mysql> SHOW VARIABLES WHERE Variable_name LIKE 'character\_set\_%' OR Variable_name LIKE 'collation%';



![img](https:////upload-images.jianshu.io/upload_images/12877758-29a32ef7c312e31e.png?imageMogr2/auto-orient/strip|imageView2/2/w/678/format/webp)



修改数据库字符集：

ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;

修改表的字符集：

ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

修改某个表的字段字符集：

ALTER TABLE table_name CHANGE column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;



#### 也可安装Navicat for MySQL工具，在工具上修改数据库和表的字符集

对应数据库修改字符集修改

![img](https:////upload-images.jianshu.io/upload_images/12877758-759bd41de09028ba.png?imageMogr2/auto-orient/strip|imageView2/2/w/959/format/webp)



对应表修改

![img](https:////upload-images.jianshu.io/upload_images/12877758-a372eaa9383384ad.png?imageMogr2/auto-orient/strip|imageView2/2/w/573/format/webp)

对应表的字符修改

![img](https:////upload-images.jianshu.io/upload_images/12877758-1c7be6178d3b35cf.png?imageMogr2/auto-orient/strip|imageView2/2/w/798/format/webp)

#### 注意：

1.需要 >= MySQL 5.5.3版本、(经检测5.5.29的也可以)低版本不支持这个字符集、复制报错

2.如果只是某个字段需要 只需要修改那个字段的字符集就可以了

3.另外服务器连接数据库 Connector/J的连接参数中，不要加characterEncoding参数。 不加这个参数时，默认值就时autodetect。

