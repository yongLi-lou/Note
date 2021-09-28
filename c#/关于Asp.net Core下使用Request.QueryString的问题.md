# 关于Asp.net Core下使用Request.QueryString的问题

在asp.net 下我们可以通过Request.QueryString["id"]来获取传入的参数，但是在asp.net core下是会报错的。

要改为HttpContext.Request.Query["id"]来获取

记录一下这个知识点