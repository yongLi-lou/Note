# 使用Quartz写定时器


### 这个类用于依赖注入注入单例然后使用_=new ()来调用
```c#
public class AutoJob : IDisposable
    {
        private static IScheduler scheduler;
        private static Task<IScheduler> TaskScheduler;
        private static readonly object Joblock = new object();

        public static async Task<IScheduler> GetSchedulerAsync()
        {
            lock (Joblock)
            {
                // 如果类的实例不存在则创建，否则直接返回
                if (scheduler == null)
                {
                    ISchedulerFactory schedf = new StdSchedulerFactory();
                    TaskScheduler = schedf.GetScheduler();
                }
                else
                {
                    return scheduler;
                }
            }
            return await TaskScheduler;
        }

        public void Dispose()
        {
            JobKey JKey = new JobKey("job", "AutoDepositRefundJob");
            TriggerKey TKey = new TriggerKey("trigger", "AutoDepositRefundJob");
            QuartzHelper.RemoveJob(JKey, TKey).Wait();
            return;
        }

        public AutoJob()
        {
            try
            {
                Task.Run(async () =>
                {
                    // 开启调度器
                    scheduler = await GetSchedulerAsync();
                    await scheduler.Start();

                    IJobDetail job1 = JobBuilder.Create<CalculateJob>()  //创建一个作业
                        .WithIdentity("job", "AutoDepositRefundJob")
                        .Build();
                    ITrigger trigger1 = TriggerBuilder.Create()
                        .WithIdentity("trigger", "AutoDepositRefundJob")
                        /*.StartNow()*/
                        //现在开始
                        .WithCronSchedule("0 0 12 * * ? *")//cron任务输入cron表达式
                        .Build();
                    await scheduler.ScheduleJob(job1, trigger1);


                });
            }
            catch (Exception)
            {
                throw;
            }
        }
    }

    public static class QuartzHelper
    {
        public static async Task RemoveJob(JobKey jobKey, TriggerKey triggerKey)
        {
            try
            {
                var scheduler = await AutoJob.GetSchedulerAsync();
                var state = await scheduler.GetTriggerState(triggerKey);
                if (state == TriggerState.None)
                {
                    return;
                }
                await scheduler.PauseTrigger(triggerKey);// 停止触发器
                await scheduler.UnscheduleJob(triggerKey);// 移除触发器
                await scheduler.DeleteJob(jobKey);// 删除任务
            }
            catch (Exception)
            {
                throw;
            }
        }
    }
```

### 这个类用于执行定时代码需要继承IJob接口

```c#
public class CalculateJob : IJob
    {
        IFreeSql _fsql;
        public CalculateJob() {
           _fsql = new FreeSqlBuilder()
                            .UseConnectionString(DataType.SqlServer, AppConfigurtaionServices.Configuration["FreeSql:ConnStr"])
                            .UseMonitorCommand(cmd => Console.WriteLine($"线程：{cmd.CommandText}\r\n"))
                            .Build();
        }

        public Task Execute(IJobExecutionContext context)
        {
            Task.Run(() => {
                //业务逻辑(算昨天的)
                var todayoee = _fsql.Select<UDP_OEE>().Where(e=>e.Date==DateTime.Now.Date.AddDays(-1)).ToList();
                if (todayoee!=null)
                {
                    int count = 0;
                    todayoee.ForEach(e => {
                        var todaycl = _fsql.Select<UDP_OEE_CL>().Where(x => e.CXBM == x.CXBM && e.Date == x.Date && e.Work == x.Work && e.GWBM == x.GWBM && e.DYGSBM == x.DYGSBM).ToOne();

                        if (todaycl!=null)
                        {
                            todaycl.Utilization = OEE_CalculateHelper.Utilization(e.Product_Time,e.Error_Time,e.Offline_Time);
                            
                            todaycl.Performance = OEE_CalculateHelper.Performance(todaycl.Pass,todaycl.UnPass,e.Product_Time);
                            todaycl.OEE = OEE_CalculateHelper.OEE(todaycl.Utilization.Value,todaycl.Performance.Value,todaycl.PassRate.Value);
                            try
                            {
                                
                                //写入数据库
                              count+=  _fsql.Update<UDP_OEE_CL>().Set(x=>x.OEE,todaycl.OEE)
                                .Set(x => x.Utilization, todaycl.Utilization)
                                .Set(x => x.Performance, todaycl.Performance)
                                .Where(x=>x.UDP_OEE_CL_ID==todaycl.UDP_OEE_CL_ID).ExecuteAffrows();
                                
                            }
                            catch (Exception x)
                            {
                                //写入日志
                                CyLogHelp.WriteLog("未知错误" + x.Message +x.InnerException, "autojob", "autojob");

                            }
                            
                        }
                        else
                        {
                            //写入日志
                            CyLogHelp.WriteLog("数据不完整UDP_OEE_CL", "autojob", "autojob");
                        }
                    });
                    //写入日志
                    CyLogHelp.WriteLog("数据库写入成功影响行数:" + count, "autojob", "autojob");
                }
                else
                {
                    //写入日志
                    CyLogHelp.WriteLog("数据不完整UDP_OEE", "autojob", "autojob");
                }
                
                
            
            });
            return Task.CompletedTask;
        }
    }
```

