# File was loaded in the wrong encoding: 'UTF-8'错误改正

File was loaded in the wrong encoding: ‘UTF-8’

下边的中文是乱码的，无论是注释中还是代码中，都是乱码的。
原因：我们文件使用UTF-8进行编辑，而Windows默认使用GBK编码格式，所以导致打开文件时出现乱码。
解决办法1
点击右下角的UTF-8，选择GBK，在弹出的窗口中选择Reload（重载）

解决办法2
在编辑文本时，设置指定的编码格式。

encoding="utf-8"
