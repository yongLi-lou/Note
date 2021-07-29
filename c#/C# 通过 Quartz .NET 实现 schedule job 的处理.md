# [C# 通过 Quartz .NET 实现 schedule job 的处理](https://www.cnblogs.com/mingmingruyuedlut/p/8037263.html)

在实际项目的开发过程中，会有这样的功能需求：要求创建一些Job定时触发运行，比如进行一些数据的同步。

那么在 .Net Framework 中如何实现这个Timer Job的功能呢？

这里所讲的是借助第三方的组件 Quartz.Net 来实现（源码位置：https://github.com/quartznet/quartznet）

详细内容请看如下步骤：

1）：首先在VS中创建一个Console Application，然后通过NuGet下载Quartz.Net组件并且引用到当前工程中。我们下载的是3.0版本，注：此版本与之前的2.0版本一定的区别。

![img](https://images2018.cnblogs.com/blog/161932/201805/161932-20180513191245633-1067520913.png)

2）：继承 IJob 接口，实现 Excute 方法

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    public class EricSimpleJob : IJob
    {
        public Task Execute(IJobExecutionContext context)
        {
            Console.WriteLine("Hello Eric, Job executed.");
            return Task.CompletedTask;
        }
    }

    public class EricAnotherSimpleJob : IJob
    {
        public Task Execute(IJobExecutionContext context)
        {
            string filepath = @"C:\timertest.txt";

            if (!File.Exists(filepath))
            {
                using (FileStream fs = File.Create(filepath)) { }
            }

            using (StreamWriter sw = new StreamWriter(filepath, true))
            {
                sw.WriteLine(DateTime.Now.ToLongTimeString());
            }

            return Task.CompletedTask;
        }
    }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

3）：完成 IScheduler, IJobDetails 与 ITrigger之间的配置

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        static async Task TestAsyncJob()
        {
            var props = new NameValueCollection
            {
                { "quartz.serializer.type", "binary" }
            };
            StdSchedulerFactory schedFact = new StdSchedulerFactory(props);

            IScheduler sched = await schedFact.GetScheduler();
            await sched.Start();

            IJobDetail job = JobBuilder.Create<EricSimpleJob>()
                .WithIdentity("EricJob", "EricGroup")
                .Build();

            IJobDetail anotherjob = JobBuilder.Create<EricAnotherSimpleJob>()
                .WithIdentity("EricAnotherJob", "EricGroup")
                .Build();

            ITrigger trigger = TriggerBuilder.Create()
                .WithIdentity("EricTrigger", "EricGroup")
                .WithSimpleSchedule(x => x.WithIntervalInSeconds(5).RepeatForever())
                .Build();

            ITrigger anothertrigger = TriggerBuilder.Create()
                .WithIdentity("EricAnotherTrigger", "EricGroup")
                .WithSimpleSchedule(x => x.WithIntervalInSeconds(5).RepeatForever())
                .Build();

            await sched.ScheduleJob(job, trigger);
            await sched.ScheduleJob(anotherjob, anothertrigger);
        }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

4）：在 Main 方法中完成调用， 由于是异步处理，因此这里用 Console.ReadKey() 完成对主线程的阻塞

```
        static void Main(string[] args)
        {
            TestAsyncJob();
            Console.ReadKey();
        }
```

5）：最终的运行结果为，两个Job使屏幕和文件不断输出字符串

![img](https://images2018.cnblogs.com/blog/161932/201805/161932-20180513192535124-1182306173.png)

更多信息请参考如下链接：

https://www.cnblogs.com/MingQiu/p/8568143.html

 

6）：如果我们想将此注册为Windows Service，在对应Service启动之后自动处理对应Job，请参考如下链接：

http://www.cnblogs.com/mingmingruyuedlut/p/9033159.html

 

如果是2.0版本的Quartz.Net请参考如下链接：

https://www.quartz-scheduler.net/download.html

https://www.codeproject.com/Articles/860893/Scheduling-With-Quartz-Net

https://stackoverflow.com/questions/8821535/simple-working-example-of-quartz-net