# [Database operation expected to affect 1 row(s) but actually affected 0 row(s). Data may have been modified or deleted since entities were loaded](https://www.cnblogs.com/liulida/p/9896488.html)

asp.net 更新数据时报错：Database operation expected to affect 1 row(s) but actually affected 0 row(s). Data may have been modified or deleted since entities were loaded

 

原因：Check your object, does it pass Primary Key `Null`. This Exception is Generally due this reason only.

对象的主键为空，导致不能更新，检查你要更新的对象，查看主键是否为空，或0

