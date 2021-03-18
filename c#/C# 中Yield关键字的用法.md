# C# 中Yield关键字的用法

ield关键字的作用是将当前集合中的元素立即返回，只要没有yield break，方法还是会继续执行循环到迭代结束。

1.返回元素用yield return;（一次一个的返回）

2.结束返回用yield break;（终止迭代）

3.返回类型必须为 IEnumerable、IEnumerable<T>、IEnumerator 或 IEnumerator<T>。

4.参数前不能使用ref和out关键字

5.匿名方法中 不能使用yield

6.unsef中不能使用

7.不能将 yield return 语句置于 try-catch 块中。 可将 yield return 语句置于 try-finally 语句的 try 块中。yield break 语句可以位于 try 块或 catch 块，但不能位于 finally 块

1 yield return 例子

using static System.Console;
using System.Collections.Generic;

class Program
{
    //一个返回类型为IEnumerable<int>，其中包含三个yield return
    public static IEnumerable<int> enumerableFuc()
    {
        yield return 1;
        yield return 2;
        yield return 3;
    }

    static void Main(string[] args)
    {
        //通过foreach循环迭代此函数
        foreach(int item in enumerableFuc())
        {
            WriteLine(item);
        }
        ReadKey();
    }
}

输出结果：
1
2
3
2，yeild break例子：

using static System.Console;
using System.Collections.Generic;
class Program
{
    //一个返回类型为IEnumerable<int>，其中包含三个yield return
    public static IEnumerable<int> enumerableFuc()
    {
        yield return 1;
        yield return 2;
        yield break;
        yield return 3;
    }

    static void Main(string[] args)
    {
        //通过foreach循环迭代此函数
        foreach(int item in enumerableFuc())
        {
            WriteLine(item);
        }
        ReadKey();
    }
}

输出结果：
1
2
————————————————
