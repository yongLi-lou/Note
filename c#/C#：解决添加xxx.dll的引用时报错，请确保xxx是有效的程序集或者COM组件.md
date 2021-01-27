# C#：解决添加xxx.dll的引用时报错，请确保xxx是有效的程序集或者COM组件

问题：

![img](https://img-blog.csdnimg.cn/20190711162047814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1NDE1NTM=,size_16,color_FFFFFF,t_70)

解决方法有两种，如下：

1.代码中用dllImport语法引入：

```cs
using System;
using System.Runtime.InteropServices;
 
class Example
{
    // Use DllImport to import the Win32 MessageBox function.
    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    public static extern int MessageBox(IntPtr hWnd, String text, String caption, uint type);
    
    static void Main()
    {
        // Call the MessageBox function using platform invoke.
        MessageBox(new IntPtr(0), "Hello World!", "Hello Dialog", 0);
    }
}
```

https://docs.microsoft.com/zh-cn/dotnet/api/system.runtime.interopservices.dllimportattribute?redirectedfrom=MSDN&view=netframework-4.7.2

 

 

2.WIN系统regsvr32命令注册dll

Regsvr32命令用于注册[C](https://baike.baidu.com/item/C)OM组件，是 Windows 系统提供的用来向系统注册控件或者卸载控件的命令，以命令行方式运行。WinXP及以上系统的[regsvr32.exe](https://baike.baidu.com/item/regsvr32.exe)在windows\system32文件夹下；2000系统的regsvr32.exe在winnt\system32文件夹下。

以管理员运行cmd

regsvr32 d:\xml\xxx.dll