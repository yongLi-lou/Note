# windbg分析内存泄漏问题

1. 收集dump文件

首先，收集在运行一段时间后的.NET应用程序的dump文件。可以使用`Task Manager`（任务管理器）或`ProcDump`工具来收集dump文件。

1. 打开dump文件

打开WinDbg，并使用"File"（文件）> "Open Crash Dump..."（打开崩溃转储文件）菜单项打开收集到的dump文件。

1. 加载.NET调试器扩展

在WinDbg中，输入以下命令以加载SOS扩展（SOS.dll）：

```

.loadby sos coreclr
```

1. 分析内存使用情况

使用以下命令分析托管堆中的对象：

```

!dumpheap -stat
```

*如果出现报错*SOS does not support the current target architecture 'x64' (0x8664). A 32 bit target may require a 32 bit debugger or vice versa. In general, try to use the same bitness for the debugger and target process.

这个错误表明您正在使用一个与目标进程不匹配的WinDbg版本。您需要使用与目标进程相同位数（32位或64位）的WinDbg。

解决这个问题的方法是确保使用与目标进程相同位数的WinDbg。在Windows调试工具包（Debugging Tools for Windows）中，有两个版本的WinDbg：

1. WinDbg (x86)：32位版本
2. WinDbg (x64)：64位版本

根据您的进程架构（32位或64位），选择正确的WinDbg版本。

### 执行成功后可以看到 MT    Count    TotalSize Class Name

该命令将显示托管堆中对象的统计信息。查找可疑的对象，例如那些数量或内存占用异常的对象。

1. 获取更多关于可疑对象的信息

使用以下命令查看指定类型的对象实例：

```

!dumpheap -type <Class Name>
```

出现三行Address       MT     Size

- Address：实例在内存中的地址。
- MT：方法表（Method Table）的地址。这是类型的内部表示。
- Size：实例占用的字节数。

将`<type-name>`替换为您在上一步中发现的可疑对象类型。这将列出该类型的所有实例。

1. 分析对象的根

选择一个可疑对象实例的地址，然后运行以下命令：

```

!gcroot <object-address>
```

将`<object-address>`替换为实际的对象地址。这将显示导致对象保持在内存中的根对象。分析这些根对象，了解为什么它们没有被垃圾回收器回收，从而找到潜在的内存泄漏问题。

1. 查看对象的值

要查看对象的字段值，可以使用以下命令：

```

!do <object-address>
```

将`<object-address>`替换为实际的对象地址。

结合以上步骤，您应该能够找出.NET应用程序的内存泄漏问题。注意，内存泄漏问题可能需要多次迭代和分析才能找到根本原因。您可能需要反复查看代码，以找出为何对象没有被正确释放。