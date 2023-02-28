# 什么是dump

Dump文件是进程的内存镜像。可以把程序的执行状态通过调试器保存到dump文件中。在 Windows 系统上， dump 文件分为内核 dump 和用户态 dump 两种。前者一般用来分析内核相关的问题，比如驱动程序；后者一般用来分析用户态程序的问题。

　　一般的程序员可能接触不到dump文件，反而是运维会用的多一些。不过如果你抗战在第一线，学会dump的分析无疑是掌握一柄利器。因为很多场景下，在线下的单元测试或者性能测试中由于测试用例的不充分或者生产与测试环境的硬件以及pv量级的不同等等情况导致问题暴露不出，而在生产环境中又没有足够的日志或者堆栈信息来指向问题产生的原因。这个时候dump文件的分析就显得很有作用。

　　正文分3节 抓取dump以及dump的手动和自动分析。对于初学者自动分析dump是很方便的一种渠道。

**一. 抓取dump**

　　1. 最简单的方法 通过任务管理器

　　![img](https://images0.cnblogs.com/i/87114/201407/301448140713419.jpg)![img](https://images0.cnblogs.com/i/87114/201407/301459076336417.jpg)

　　![img](https://images0.cnblogs.com/i/87114/201407/301459223522621.jpg)

　　2. 通过debugdiag

　　debugdiag是一个微软提供的dump抓取和分析工具。可以建立各种规则在不同的条件下抓取dump，同时具有强大的dump分析功能。

　　下载地址：http://www.microsoft.com/en-us/download/details.aspx?id=26798

　　![img](https://images0.cnblogs.com/i/87114/201407/301503142909461.jpg)

　　![img](https://images0.cnblogs.com/i/87114/201407/301534370244277.jpg)

　　3. Adplus方式

　　运行 cmd ，进入 adplus.exe 文件所在目录，运行如下命令：
　　单个进程： adplus .exe – hang – p <PID> – o d: ¥
　　多个进程： adplus .exe – hang – p <PID1> -p <PID2> – o d: ¥
　　Mini Dump ： adplus .exe - MiniOnSecond – hang – p <PID> – o d: ¥　

　　**抓取方式的选择：**

　　任务管理器的抓取适合dump文件不大，对应系统盘默认存放路径的空间完全足够的情况。

　　debugdiag的抓取可以适应多种情况，通过工具的配置来完成。

　　Adplus解决了任务管理器抓取方式的限制，可以处理对应多个进程大文件的情况。

**二. dump的手动分析**

　　工具： winbdg

　　WinDBG不是专门用于调试.Net程序的工具，它更偏向于底层，可用于内核和驱动调试。进行普通的.Net程序调试还是使用微软专为.Net开发的调试工具MDBG更方便一些。但是WinDBG能看到更多的底层信息，对于某些特别疑难的问题调试有所帮助，例如内存泄漏等问题。

　　工具下载： [WinBdgTool.zip](http://pan.baidu.com/s/1ntC2K1V)

　　测试代码下载 ： [MyDumpTest.7z](https://files.cnblogs.com/dubing/MyDumpTest.7z)

　　首先添加设定符号文件路径（Symbol Path）,当你使用Visual Studio编译程序时，是否有留意到在bin/Debug文件夹下会有.pdb后缀的文件？这些文件包含有dll程序集的调试符号，pdb文件并不包含有执行代码，只是使调试工具能把代码执行指令翻译为正确的可识别字符。微软提供了包含大量pdb文件的公共服务器，地址如下：http://msdl.microsoft.com/download/symbols。打开windbg程序，选择“File->Symbol File Path…“，把下面的内容复制进去保存。srv*c:\temp*http://msdl.microsoft.com/download/symbols。

　　![img](https://images0.cnblogs.com/i/87114/201407/301649271027966.jpg)

　　下面这行命令 如果你发现出现Unable to verify checksum...或者的消息 那是因为你没有添加.net的sos扩展或者sos的版本没有对应上。.Net1.1时代的SOS扩展已经自带于下载安装的WinDBG中，从.Net2.0以后，SOS扩展已经自带到.Net框架中：C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\SOS.dll，为了不至于引起混淆，最好的方法就是使用前面的loadby调试器元命令来让WinDBG自己决定加载什么版本的SOS。

　　添加sos：.load C:\WINDOWS\Microsoft.NET\Framework\v4.0.30319\sos.dll。

　　**加载SOS后，使用命令.chain来查看调试链中是否已经成功包含SOS扩展。**

　　![img](https://images0.cnblogs.com/i/87114/201407/301705555081802.jpg)

　　**通过!eeversion查看sos的版本号。**

　　![img](https://images0.cnblogs.com/i/87114/201407/301706480872506.jpg)

　　**实战命令： ~ 查看线程**

　　![img](https://images0.cnblogs.com/i/87114/201407/301658551801915.jpg)

　　这表明当前dump里记录的线程数。如果要切换线程，用波浪线+序号+s来切换，如切换到线程2，那么用～2s即可。

　　**lm 查看你加载的模块**

　　**![img](https://images0.cnblogs.com/i/87114/201407/301710063999087.jpg)**

　　**kb 查看native code调用栈**

　　用~现在只有线程信息，对于每个线程，在被抓的那一刻，在执行什么，我们有命令：kb。

　　![img](https://images0.cnblogs.com/i/87114/201407/301702043524089.jpg)

　　看到clr大家应该很眼熟吧。这里已经可以看到较详细的调试信息了。

　　**!runaway   (查看线程对应 CPU 运行时间)**

　　![img](https://images0.cnblogs.com/i/87114/201407/301712492274309.jpg)

　　因为我们的测试程序是测试的是线程阻塞所以我们选一个运行时间为0的，例如415

　　![img](https://images0.cnblogs.com/i/87114/201407/301715006495926.jpg)

　　**!dso 查看这个堆栈中的对象**

　　**![img](https://images0.cnblogs.com/i/87114/201407/301720366961062.jpg)**

　　**!clrstack 查看这个线程的托管代码调用栈**

　　**![img](https://images0.cnblogs.com/i/87114/201407/301727546497664.jpg)**

　　通过上面我们已经可以看出这个线程一直都是处于阻塞状态。

　　到这里基本上一个小的测试程序可以告一段落了，当然windbg的功能远远不止如此，这里分享一些资源给大家。

　　资源下载 ： [WinDbg入门.rar](https://files.cnblogs.com/dubing/WinDbg入门.rar) [Windbg用法详解.7z](https://files.cnblogs.com/dubing/Windbg用法详解.7z)

------

 **三. dump的自动分析**

　　1. debugdiag

　　![img](https://images0.cnblogs.com/i/87114/201407/301531220878364.jpg)

　　![img](https://images0.cnblogs.com/i/87114/201407/301535311021723.jpg)

　　这里有几种规则类型的选择，一般我们常用的用crash来查看锁和堵塞的情况，performance来检查性能的问题。

　　选择完成后直接点击开始分析

　　![img](https://images0.cnblogs.com/i/87114/201407/301537013529683.jpg)

　　生成报表

　　![img](https://images0.cnblogs.com/i/87114/201407/301747538686228.jpg)

　　查看描述

　　![img](https://images0.cnblogs.com/i/87114/201407/301749236802491.jpg)

　　点击详细

　　![img](https://images0.cnblogs.com/i/87114/201407/301552455718853.jpg)

　　这样，红色字体就是问题的所在。然后根据具体问题下发到对应开发部门解决。

　　2. Hang自动化分析

　　在WinDbg输入如下命令

　　*.shell -ci "~\* kb;.echo MANAGED THREADS;!threads;.echo MANAGED CALLSTACKS;~\* e !clrstack;" D:\xx.exe*