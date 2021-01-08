# System.Diagnostics.Stopwatch

**测量一个时间间隔的运行时间**

a.调用 [Start](http://msdn.microsoft.com/zh-cn/library/system.diagnostics.stopwatch.start(VS.80).aspx) 方法

b.调用 [Stop](http://msdn.microsoft.com/zh-cn/library/system.diagnostics.stopwatch.stop(VS.80).aspx) 方法

c.使用 [Elapsed](http://msdn.microsoft.com/zh-cn/library/system.diagnostics.stopwatch.elapsed(VS.80).aspx) 属性检查运行时间。

如：

`System.Diagnostics.Stopwatch stopwatch = new System.Diagnostics.Stopwatch();`

`stopwatch.Start();`

`stopwatch.Stop();`

`stopwatch.ElapsedMilliseconds.ToString();`

`stopwatch.Reset();`



