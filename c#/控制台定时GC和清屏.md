# 控制台定时GC和清屏

```c#
///<summary>
/// 定时清理内存
/// </summary>
static void SetTimer()
{
    System.Timers.Timer aTimer = new System.Timers.Timer(); //初始化定时器
    aTimer.Interval = 60000;//配置时间1分钟
    aTimer.Elapsed += new System.Timers.ElapsedEventHandler(OnTimedEvent);
    aTimer.AutoReset = true;//每到指定时间Elapsed事件是到时间就触发
    aTimer.Enabled = true; //指示 Timer 是否应引发 Elapsed 事件。
}
//定时器触发的处理事件
static void OnTimedEvent(Object source, ElapsedEventArgs e)
{

    //清理内存
    GC.Collect();
    GC.WaitForPendingFinalizers();
    Console.Clear();
    Console.WriteLine("GC回收");
}
```

在主方法中调用SetTimer（）