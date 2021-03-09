# 多线程下C#如何保证线程安全?

多线程编程相对于单线程会出现一个特有的问题，就是线程安全的问题。所谓的线程安全，就是如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的。 线程安全问题都是由全局变量及静态变量引起的。

　　为了保证多线程情况下，访问静态变量的安全，可以用锁机制来保证,如下所示：

```c#
//需要加锁的静态全局变量
        private static bool _isOK = false;
        //lock只能锁定一个引用类型变量
        private static object _lock = new object();
        static void MLock()
        {
            //多线程
            new System.Threading.Thread(Done).Start();
            new System.Threading.Thread(Done).Start();
            Console.ReadLine();
        }

        static void Done()
        {
            //lock只能锁定一个引用类型变量
            lock (_lock)
            {
                if (!_isOK)
                {
                    Console.WriteLine("OK");
                    _isOK = true;
                }
            }
        }
```

需要注意的是，Lock只能锁住一个引用类型的对象。另外，除了锁机制外，高版本的C#中加入了async和await方法来保证线程安全，如下所示：

```c#
public static class AsynAndAwait
 {
        //step 1
        private static int count = 0;
        //用async和await保证多线程下静态变量count安全
        public async static void M1()
        {
            //async and await将多个线程进行串行处理
            //等到await之后的语句执行完成后
            //才执行本线程的其他语句
            //step 2
            await Task.Run(new Action(M2));
            Console.WriteLine("Current Thread ID is {0}", System.Threading.Thread.CurrentThread.ManagedThreadId);
            //step 6
            count++;
            //step 7
            Console.WriteLine("M1 Step is {0}", count);
        }

        public static void M2()
        {
            Console.WriteLine("Current Thread ID is {0}", System.Threading.Thread.CurrentThread.ManagedThreadId);
            //step 3
            System.Threading.Thread.Sleep(3000);
            //step 4
            count++;
            //step 5
            Console.WriteLine("M2 Step is {0}", count);
        }
}
```

