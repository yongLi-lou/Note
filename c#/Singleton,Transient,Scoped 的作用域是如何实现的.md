# Singleton,Transient,Scoped 的作用域是如何实现的

一：背景
1. 讲故事
前几天有位朋友让我有时间分析一下 aspnetcore 中为什么向 ServiceCollection 中注入的 Class 可以做到 Singleton，Transient，Scoped，挺有意思，这篇就来聊一聊这一话题，自从 core 中有了 ServiceCollection, 再加上流行的 DDD 模式，相信很多朋友的项目中很少能看到 new 了，好歹 spring 十几年前就是这么干的。

二：Singleton,Transient,Scoped 基本用法
分析源码之前，我觉得有必要先介绍一下它们的玩法，为方便演示，我这里就新建一个 webapi 项目，定义一个 interface 和 concrete ，代码如下：

    public class OrderService : IOrderService
    {
        private string guid;
    
        public OrderService()
        {
            guid = $"时间:{DateTime.Now}, guid={ Guid.NewGuid()}";
        }
    
        public override string ToString()
        {
            return guid;
        }
    }
    
    public interface IOrderService
    {
    }

1. AddSingleton
正如名字所示它可以在你的进程中保持着一个实例，也就是说仅有一次实例化，不信的话代码演示一下哈。


    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();
    
            services.AddSingleton<IOrderService, OrderService>();
        }
    }
    
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        IOrderService orderService1;
        IOrderService orderService2;
    
        public WeatherForecastController(IOrderService orderService1, IOrderService orderService2)
        {
            this.orderService1 = orderService1;
            this.orderService2 = orderService2;
        }
    
        [HttpGet]
        public string Get()
        {
            Debug.WriteLine($"{this.orderService1}\r\n{this.orderService2} \r\n ------");
            return "helloworld";
        }
    }

接着运行起来多次刷新页面，如下图：



可以看到，不管你怎么刷新页面，guid都是一样，说明确实是单例的。

2. AddScoped
正从名字所述：Scope 就是一个作用域，那在 webapi 或者 mvc 中作用域是多大呢？ 对的，就是一个请求，当然请求会穿透 Presentation, Application, Repository 等等各层，在穿层的过程中肯定会有同一个类的多次注入，那这些多次注入在这个作用域下维持的就是单例，如下代码所示：


        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();
    
            services.AddScoped<IOrderService, OrderService>();
        }

运行起来多次刷新页面，如下图：



很明显的看到，每次刷 UI 的时候，guid都会变，而在同一个请求 (scope) 中 guid 是一样的。

3. AddTransient
前面大家也看到了，要么作用域是整个进程，要么作用域是一个请求，而这里的 Transient 就没有作用域概念了，注入一次 实例化一次，不信的话上代码给你看呗。


        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();
    
            services.AddTransient<IOrderService, OrderService>();
        }



从图中可以看到，注入一次就 new 一次，非常简单吧，当然了，各有各的应用场景。

之前不清楚的朋友到现在应该也明白了这三种作用域，接下来继续思考的一个问题就是，这种作用域是如何做到的呢？ 要想回答这个问题，只能研究源代码了。

三：源码分析
aspnetcore 中的 IOC 容器是 ServiceCollection，你可以向 IOC 中注入不同作用域的类，最后生成 provider，如下代码所示：


            var services = new ServiceCollection();
    
            services.AddSingleton<IOrderService, OrderService>();
    
            var provider = services.BuildServiceProvider();

1. AddSingleton 的作用域是如何实现的
通常说到单例，大家第一反应就是 static，但是一般 ServiceCollection 中会有成百上千个 AddSingleton 类型，都是静态变量是不可能的，既然不是 static，那就应该有一个缓存字典什么的，其实还真的有这么一个。

1）RealizedServices 字典
每一个 provider 内部都会有一个 叫做 RealizedServices 的字典，这个 字典 将会在后面充当缓存存在， 如下图：



从上图中可以看到，初始化的时候这个字典什么都没有，接下来执行 var orderService = provider.GetService<IOrderService>(); 效果如下图：



可以看到 RealizedServices 中已经有了一个 service 记录了，接着往下执行 var orderService2 = provider.GetService<IOrderService>();，最终会进入到 CallSiteRuntimeResolver.VisitCache 方法判断实例是否存在，如下图：



仔细看上面代码的这句话: if (!resolvedServices.TryGetValue(callSite.Cache.Key, out obj)) 一旦字典存在就直接返回，否则就要执行 new 链路，也就是 this.VisitCallSiteMain。

综合来看，这就是为什么可以单例的原因，如果不明白可以拿 dnspy 仔细琢磨琢磨。。。

2. AddTransient 源码探究
前面大家也看到了，provider 里面会有一个 DynamicServiceProviderEngine 引擎类，引擎类中用 字典缓存 来解决单例问题，可想而知，AddTransient 内部肯定是没有字典逻辑的，到底是不是呢？ 调试一下呗。



和单例一样，最终解析都是由 CallSiteRuntimeResolver 负责的，AddTransient 内部会走到 VisitDisposeCache 方法，而这里会一直走 this.VisitCallSiteMain(transientCallSite, context) 来进行 实例的 new 操作，还记得单例是怎么做的吗？ 它会在这个 VisitCallSiteMain 上包一层 resolvedServices 判断，🤭 继续追一下 VisitCallSiteMain 方法吧，这个方法最终会走 CallSiteKind.Constructor 分支调用你的构造函数,代码如下：


        protected virtual TResult VisitCallSiteMain(ServiceCallSite callSite, TArgument argument)
        {
            switch (callSite.Kind)
            {
            case CallSiteKind.Factory:
                return this.VisitFactory((FactoryCallSite)callSite, argument);
            case CallSiteKind.Constructor:
                return this.VisitConstructor((ConstructorCallSite)callSite, argument);
            case CallSiteKind.Constant:
                return this.VisitConstant((ConstantCallSite)callSite, argument);
            case CallSiteKind.IEnumerable:
                return this.VisitIEnumerable((IEnumerableCallSite)callSite, argument);
            case CallSiteKind.ServiceProvider:
                return this.VisitServiceProvider((ServiceProviderCallSite)callSite, argument);
            case CallSiteKind.ServiceScopeFactory:
                return this.VisitServiceScopeFactory((ServiceScopeFactoryCallSite)callSite, argument);
            }
            throw new NotSupportedException(string.Format("Call site type {0} is not supported", callSite.GetType()));
        }
    最终由 VisitConstructor 对我的实例代码的构造函数进行调用，所以你应该理解了为啥每次注入都会new一次。如下图：


3. AddScoped 源码探究
当你明白了 AddSingleton， AddTransient 的原理，我想 Scoped 也是非常容易理解的，肯定是一个 scoped 一个 RealizedServices 对吧，不信的话继续上代码哈。


        static void Main(string[] args)
        {
            var services = new ServiceCollection();
    
            services.AddScoped<IOrderService, OrderService>();
    
            var provider = services.BuildServiceProvider();
    
            var scoped1 = provider.CreateScope();
    
            var scoped2 = provider.CreateScope();
    
            while (true)
            {
                var orderService = scoped1.ServiceProvider.GetService<IOrderService>();
    
                var orderService2 = scoped2.ServiceProvider.GetService<IOrderService>();
    
                Console.WriteLine(orderService);
    
                Thread.Sleep(1000);
            }
        }

然后看一下 scoped1 和 scoped2 是不是都存在独立的缓存字典。



从图中可以看到，scoped1 和 scoped2 中的 ResolvedServices 拥有不用的count，也就说明两者是独立存在的，相互不影响。

四： 总结
很多时候大家都这么习以为常的用着，突然有一天被问起还是有点懵逼的，所以时常多问自己几个为什么还是很有必要的哈😄😄😄。

原文：https://blog.csdn.net/huangxinchen520/article/details/108334554