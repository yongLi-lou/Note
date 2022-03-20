# 在linux环境下执行一下代码时报-bash: !": event not found
在linux环境下执行一下代码时，如下例子：
FreeSql.Generator -Razor "__razor.cshtml.txt" -NameOptions 0,0,0,0 -NameSpace UDP_BYD_MODEL -DB "SqlServer,data source=47.98.247.221;initial catalog=UDP_BYD;user id=sa;password=sql@Nbcy1234!;pooling=true;max pool size=2;Encrypt=True;TrustServerCertificate=True;" -FileName "{name}.cs"
返回结果：
-bash: !: event not found

原因为：
输入的命令中间包含 ！,叹号，不能组成命令, 应该将 ！转义，加上“\”反转意符号即可解决，其他shell命令出现类似问题可以同样的方式解决。
FreeSql.Generator -Razor "__razor.cshtml.txt" -NameOptions 0,0,0,0 -NameSpace UDP_BYD_MODEL -DB "SqlServer,data source=47.98.247.221;initial catalog=UDP_BYD;user id=sa;password=sql@Nbcy1234\!;pooling=true;max pool size=2;Encrypt=True;TrustServerCertificate=True;" -FileName "{name}.cs"


