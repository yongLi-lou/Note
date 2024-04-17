# 字符串的逻辑表达式如何判断

使用了`System.Linq.Dynamic.Core`库来解析和执行动态表达式：

C#



```csharp
using System;
using System.Linq.Dynamic.Core;
using System.Collections.Generic;

public class Entity
{
    public int a { get; set; }
    public string b { get; set; }
}

public class Program
{
    public static void Main()
    {
        Entity entity = new Entity { a = 10, b = "test" };
        Console.WriteLine(ConditionCheck(entity, "a > 0 && a < 20 && b == \"test\""));
        Console.WriteLine(ConditionCheck(entity, "a == (2 * 5) && b == \"test\""));
    }

    public static bool ConditionCheck(Entity entity, string condition)
    {
        var config = new ParsingConfig { AllowNewToEvaluateAnyType = true };
        var parameters = new List<ParameterExpression>
        {
            Expression.Parameter(typeof(Entity), "entity")
        };

        var e = DynamicExpressionParser.ParseLambda(config, parameters.ToArray(), typeof(bool), condition);
        return (bool)e.Compile().DynamicInvoke(entity);
    }
}
```

```c#
using System;
using System.Linq.Dynamic.Core;
using System.Collections.Generic;

public class Program
{
    public static void Main()
    {
        var dict = new Dictionary<string, object> { { "a", 10 }, { "b", "test" } };
        Console.WriteLine(ConditionCheck(dict, "Convert.ToInt32(a) > 0 && Convert.ToInt32(a) < 20 && b == \"test\""));
        Console.WriteLine(ConditionCheck(dict, "Convert.ToInt32(a) == (2 * 5) && b == \"test\""));
    }

    public static bool ConditionCheck(Dictionary<string, object> dict, string condition)
    {
        var config = new ParsingConfig { AllowNewToEvaluateAnyType = true };
        var parameters = new List<ParameterExpression>
        {
            Expression.Parameter(typeof(Dictionary<string, object>), "dict")
        };

        var e = DynamicExpressionParser.ParseLambda(config, parameters.ToArray(), typeof(bool), condition);
        return (bool)e.Compile().DynamicInvoke(dict);
    }
}

```



在这个示例中，`ConditionCheck`方法接受一个`Entity`对象和一个条件字符串。它使用`DynamicExpressionParser.ParseLambda`方法来解析条件字符串，并创建一个lambda表达式。这个表达式可以包含复杂的逻辑，包括数字的加减乘除和比较运算符，以及字符串的等于判断。