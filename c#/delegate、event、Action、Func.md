# C#中的delegate、event、Action、Func

都属于委托，只是展现的形式不同而已，无论哪种，其实都可以采用delegate实现，为什么会出现另外的三种呢？

　　因为delegate是很宽泛的，格式内容都不受限，俗话说没有规矩不成方圆，如果一人过于随意，那么他所做的事也规范不到哪去，这就会导致后期的维护很费劲，实际开发中也基本都用后面三种。

区别：

　　delegate：至少0个参数，至多32个参数，可以无返回值，也可以指定返回值类型。

　　Action：无返回值的泛型委托。

       Action 表示无参，无返回值的委托
　　 Action<int,string> 表示有传入参数int,string无返回值的委托
 　　Action<int,string,bool> 表示有传入参数int,string,bool无返回值的委托
       Action<int,int,int,int> 表示有传入4个int型参数，无返回值的委托
　　 Action至少0个参数，至多16个参数，无返回值。
　　Func：有返回值的泛型委托

        Func<int> 表示无参，返回值为int的委托
　　 Func<object,string,int> 表示传入参数为object, string 返回值为int的委托
　　 Func<object,string,int> 表示传入参数为object, string 返回值为int的委托
　　 Func<T1,T2,,T3,int> 表示传入参数为T1,T2,,T3(泛型)返回值为int的委托
　　 Func至少0个参数，至多16个参数，根据返回值泛型返回。必须有返回值，不可voidevent：无需定义委托，直接使用
event关键修饰的委托，只能在该类中调用，主要用于封装安全。

        public class Dog {
            public event Action HelloEvent;
     
            public void SayHello()
            {
                HelloEvent();
            }
        }
外部代码：

            Dog dog = new Dog();
            dog.HelloEvent += say1;
            dog.SayHello();
        static void say1()
        {
            Console.WriteLine("say1");
        }
而且只能+=和-=操作

使用委托可以实现类似多肽功能：

         static string ToUpper(string str)
        {
            return str.ToUpper();
        }
        static string ToLower(string str)
        {
            return str.ToLower();
        }
        static string ToStar(string str)
        {
            return str + "星";
        }
            
        static void DealString(Func<string, string> dealFunc,string str)
        {
            Console.WriteLine(dealFunc(str));
        }
            DealString(ToUpper, "Abc");
            DealString(ToLower, "aBc");
            DealString(ToStar, "Abc");

