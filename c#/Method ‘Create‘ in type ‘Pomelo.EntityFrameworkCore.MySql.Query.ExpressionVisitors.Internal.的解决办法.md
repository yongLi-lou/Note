# Method ‘Create‘ in type ‘Pomelo.EntityFrameworkCore.MySql.Query.ExpressionVisitors.Internal.的解决办法

在.net core 3.1项目中使用EF连接MySql数据库，因为EntityFrameworkCore版本过高，引发一下错误
Method ‘Create’ in type ‘Pomelo.EntityFrameworkCore.MySql.Query.ExpressionVisitors.Internal.MySqlSqlTranslatingExpressionVisitorFactory’ from assembly ‘Pomelo.EntityFrameworkCore.MySql, Version=3.1.1.0, Culture=neutral, PublicKeyToken=2cc498582444921b’ does not have an implementation.

解决办法：将EntityFrameworkCore和Pomelo.EntityFrameworkCore.MySql的版本全部降回3.x版本即可。
