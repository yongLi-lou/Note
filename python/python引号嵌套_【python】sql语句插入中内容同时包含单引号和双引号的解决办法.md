# python引号嵌套_【python】sql语句插入中内容同时包含单引号和双引号的解决办法...

在python中调用MySQLdb模块插入数据信息，假设待输入信息data为：

Hello'World"!

其中同时包含了单引号和双引号

一般插入语句为

sql = "insert into tb (my_str) values('%s')" % (data)

cursor.execute(sql)

其中values('%s')中的%s外面也要有引号，这个引号与data中的引号匹配导致了内容错误

解决办法一: MySQLdb.escape_string()

在MySQLdb模块中自带针对mysql的转义函数escape_string()，直接调用即可

sql = "insert into tb (my_str) values('%s')" % (MySQLdb.escape_string(data))

cursor.execute(sql)

解决办法二：转义字符

将data变为下面的形式,再插入数据库就正确了

Hello\'World\"!

具体在python中的转义函数如下：

def transferContent(self, content):

if content is None:

return None

else:

string = ""

for c in content:

if c == '"':

string += '\\\"'

elif c == "'":

string += "\\\'"

elif c == "\\":

string += "\\\\"

else:

string += c

return string

要加三个\，这是因为\\会在函数中转义为\，\'会转义成'，两者合起来才能在字符串中留下 \'，然后sql读取后才能识别这是转义

注意，\本身也需要转义，否则如果原本字符串中为\',只转义'就会变成\\\\',结果是\\\\相互抵消，只剩下'

在python中，下面两种写法是一样的

a=" ' "

a=" \' "

菜菜小问题——python中print函数 以及单引号、双引号、三引号

直接面对——引号,就是为了保证打印出来的东东符合预期 如:print("小菜菜") 结果是: .================1========================= ...

【Oracle】oracle10g以后利用q-quote特性简化包含单引号后双引号的字符串写法

The Q-quote delimiter can be any single- or multibyte character except space, tab, and return. If th ...

python 单引号、双引号和三引号混用

单引号: 当单引号中存在单引号时,内部的单引号需要使用转义字符,要不然就会报错: 当单引号中存在双引号时,双引号可以不用加转义字符,默认双引号为普通的字符,反之亦然. 双引号: 当双引号中存在双引号时 ...

PHP 单引号与双引号的区别 SQL中的使用

php单引号与双引号用法:引号嵌套方法 1.双引号内不能直接就再嵌套双引号 2.双引号与单引号互相嵌套使用 如: 双引号内直接嵌套单引号 echo "

一.引号定义字符串 在Php中,通常一个字符串被定义在一对引号中,如: 'I am a string in single quotes'"I am a string in double qu ...

python 字符串组成MySql 命令时，字符串含有单引号或者双引号导致出错解决办法

引用自:https://blog.csdn.net/zhaoya_huangqing/article/details/48036839 一.在组成SQL语句并发送命令时完全按照Python中的样式去传 ...

php中的单引号、双引号和转义字符

PHP单引号及双引号均可以修饰字符串类型的数据,如果修饰的字符串中含有变量(例$name):最大的区别是: 双引号会替换变量的值,而单引号会把它当做字符串输出. 例如: <?php     ...

shell中的括号(小括号，中括号，大括号)及单引号、 双引号，反引号(&grave;&grave;)

一.小括号,园括号() 1.单小括号 () ①命令组.括号中的命令将会新开一个子shell顺序执行,所以括号中的变量不能够被脚本余下的部分使用.括号中多个命令之间用分号隔开,最后一个命令可以没有分号, ...

java中的单引号和双引号

1.单引号引的数据 是char类型的,双引号引的数据 是String类型的:单引号只能引一个字符,而双引号可以引0个及其以上.char只是一个基本类型,而String 可以是一个类,可以直接引用.比如 ...

随机推荐

Sublime Text快捷键和常用插件推荐

Sublime Text快捷键: Ctrl+Shift+P:打开命令面板 Ctrl+P:搜索项目中的文件 Ctrl+G:跳转到第几行 Ctrl+W:关闭当前打开文件 Ctrl+Shift+W:关闭所有 ...

Android Studio-设置switch&sol;case代码块自动补齐

相信很多和我一样的小伙伴刚从Eclipse转到Android Studio的时候,一定被快捷键给搞得头晕了,像Eclipse中代码补齐的快捷键是Alt+/ ,但是在AS中却要自己设置,这还不是问题的关 ...

Android Activity学习笔记(一)

Activity为Android应用的四大组件之一,提供界面来与用户完成交互等操作.其中Activity的生命周期的知识这里做个笔记. Activity的生命周期由以下几个部分组成: 1.onCrea ...

&period;net批量删除和添加

往页面上拖一个GridView,设置好数据源,并为GridView添加一个模板列,往模板列里添加一个chekcbox,比如下面的代码

10倍速！一招儿解决因googleapis被墙导致的许多国外网站访问速度慢的问题

1x.com 是我非常喜欢的一家国外的摄影网站.但,打开它的首页要1分多钟!点击小图看大图的二级页面根本打不开.看着写着“Nude content”的小图却点不开大图的心情你们造吗?!很多国外网站访问 ...

清北学堂学习总结day1

上午篇 一.高精度计算: [以下内容先只考虑非负数情况] •高精度加法: 思路:[模拟竖式运算] 注意:[进位] •高精度减法: 思路:[同加法类似,模拟竖式运算,进位变退位] 