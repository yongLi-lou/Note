# C#中SemaphoreSlim的使用

```c#
// 现在有10个人要过桥
// 但是一座桥上只能承受5个人，再多桥就会塌
public static void SemaphoreTest()
{
    var semaphore = new SemaphoreSlim(5);
    for (int i = 1; i <= 10; i++)
    {
        Thread.Sleep(100); // 排队上桥
        var index = i; // 定义index 避免出现闭包的问题
        Task.Run(() =>
        {
            semaphore.Wait();
            try
            {
                Console.WriteLine($"第{index}个人正在过桥。");
                Thread.Sleep(5000); // 模拟过桥需要花费的时间
            }
            finally
            {
                Console.WriteLine($"第{index}个人已经过桥。");
                semaphore.Release();
            }
        });
    }
}
```

