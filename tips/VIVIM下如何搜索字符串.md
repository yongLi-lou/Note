# [VI/VIM下如何搜索字符串](https://www.cnblogs.com/ningff/p/12077866.html)

\1. 命令模式下，输入：/字符串

比如搜索user, 输入/user

按下回车之后，可以看到vim已经把光标移动到该字符处和高亮了匹配的字符串

 

\2. 查看下一个匹配，按下n(小写n)

 

\3. 跳转到上一个匹配，按下N（大写N）

 

\4. 搜索后，我们打开别的文件，发现也被高亮了，怎么关闭高亮？

​    命令模式下，输入:nohlsearch  也可以:set nohlsearch； 当然，可以简写，noh或者set noh。