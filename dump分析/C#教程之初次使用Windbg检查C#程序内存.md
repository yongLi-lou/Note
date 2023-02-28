- ## C#教程之初次使用Windbg检查C#程序内存

选择 File - Attach to a process，然后在弹出的窗口中选择我们正在运行的控制台程序

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T31443-3.png)

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T36464-4.png)

### 6. 加载 sos 工具

如下图。然后输入 .chain，用于确认sos.dll确实被加载。

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T3B21-5.png)

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T35291-6.png)

### 7. 获得主线程上的引用

输入命令： ~0s 

 ![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T33a0-7.png)

### 8. 输出主线程上的线程栈信息

输入命令 !clsstack -l 。可以看到，main方法有一个局部变量，地址是 0x020c2350

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T31505-8.png)

### 9. 输出局部变量的信息

!dumpobj /d 0x020c2350 。如下图

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T3IM-9.png)

打印出来的内容：

Name：类名

MethodTable：方法表地址

Size：占用内存空间多少字节

红色框里，是该对象所有字段的详细信息表格，包含每个字段的MT（方法表地址）、Offset（相对偏移量）、类型、VT（=1：值类型，=0：引用类型）、Attr（静态的还是实例的）、字段值、字段名称

注意，在offset中，可以看到每个字段在内存的分布，如下图。

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T36104-10.png)

 

这么分布是为了字段对齐。即，CLR为了节省空间，各个字段在内存中，并不是按代码定义的顺序进行分布的，而是把byte字段合到一起，让它们共同占用4个字节。int每个变量占4个字节。就形成了上图的分布。

### 10. 查看内存情况

选择菜单 View - Memory，在Virtual中输入刚才变量地址，可以验证上面所说的情况

![img](https://www.xin3721.com/articlelist/uploads/allimg/190413/192T32M8-11.png)