# [使用Aspose.Cells利用模板导出Excel（C#）](https://www.cnblogs.com/liuyoung/p/7736811.html)

 传统的使用Microsoft.Office.Interop 或者 Microsoft.ACE.OLEDB 都具有一些使用限制：

- 需要在服务器端装Excel或者Microsoft.ACE.OLEDB，且及时更新它，以防漏洞，还需要设定权限允许.NET访问COM+，如果在导出过程中出问题可能导致服务器宕机。
- Excel会把只包含数字的列进行类型转换，本来是文本型的，Excel会将其转成数值型的，比如编号000123会变成123。
- Excel会根据Excel文件前8行分析数据类型，如果正好你前8行某一列只是数字，那它会认为该列为数值型，自动将该列转变成类似1.42702E+17格式，日期列变成包含日期和数字的。
- 导出时，如果字段内容以“-”或“=”开头，Excel会把它当成公式进行，会报错。

  C#中有很多第三方组件支持导出Excel，比如：NPOI、Aspose.Cells以及Spire.xls等等。它们能克服Microsoft.Office.Interop 或者 Microsoft.ACE.OLEDB的这些缺点。这里我们使用Aspose.Cells，同时使用已经写好的模板。

# 一、准备数据库

我创建了一个非常简单的表格,添加了一些数据。结构如图所示：

![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026142354305-764028477.png)

 

# 二、创建Excel模板

新建一个excel文件，第一行为标题，第二行添加内容。第二行格式为：&=[数据源表格名称].列名称。其中“数据源表格名称”为后台返回DataTable的名称，“列名称”为对应的标题列在数据库中的名称。具体如下：

![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026143407242-159935429.png)

# 三、后台写导出Excel的方法

具体代码如下：

![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026143827273-1605672775.png)

然后在控制器中写个方法，调用ExportExcel。这个方法供前台js调用。

![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026144234586-1151343023.png)

# 四、视图中调用

 使用js调用控制器中的方法，要注意不能使用ajax。只能使用window.location.href。

 ![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026144508008-298511909.png)

# 五、实现效果

点击页面上的“导出”按钮，会弹出文件保存对话框。效果如下：

![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026144905789-428019010.png)

打开Excel，看看里面的内容：

![img](https://images2017.cnblogs.com/blog/1185327/201710/1185327-20171026144959055-1125614323.png)

数据库表中的数据，都填充在了Excel中。实现了我们想要的效果。

# 六、结语

本次分享到此结束。如果这篇文章对你有帮助的话,评论或推荐下吧！