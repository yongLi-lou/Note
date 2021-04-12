# VS断点进不去解决方案

1. 确认是否是debug模式

2. （1）工具 --> 选项 --> 调试 --> 常规

   （2）不勾选下图中蓝线所画选项。

   ![img](https://img-blog.csdnimg.cn/20181115090512893.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2l0d29ybGQxMjM=,size_16,color_FFFFFF,t_70)

   2、其他

   exe使用的dll文件与你现在这个工程调试所用的dll文件不是同一个文件，因为不同时期生成的dll，即使代码一样，其内部标识符也不一样，故需要二者统一才行。