# 表达式树

#### 表达式树是什么有什么用
> Expression属于System.Linq.Expression命名空间下。

> ask：Expression是什么？ 

> answer：Expression是表达式树 

> ask：表达式树用来干什么？

> answer：表达式树提供一个将可执行代码转换成数据结构的方法，我们可以把它理解为一种数据结构。

> ask：可执行代码指什么？

> answer：可执行代码其实就是指C#代码，例：lambda表达式是可执行代码、LINQ查询表达式也是可执行代码 

> ask：表达式树价值体现在什么地方？

> answer：在LINQ中，我们使用Linq查询表达式来从数据库中获取数据，很显示，数据库并不认识LINQ语法，它只认识SQL语句，这时表达式树就派上用场了，它将LINQ查询表达式解析成一个数据结构，再对数据结构进行分析，进而组合成SQL数据库需要的SQL语句。

#### 变量表达式

在表达式树中使用ParameterExpression或者ParameterExpression表达式表示变量类型，下面看一个例子，我们定义一个int类型的变量i：

 `// ParameterExpression表示命名的参数表达式。
 ParameterExpression i = Expression.Parameter(typeof(int),"i");`

或者使用

`ParameterExpression j = Expression.Variable(typeof(int), "j");`

通过<kbd>f12</kbd>转到定义，发现这两个方法的注释几乎是一样的。静态方法Parameter第一个参数：定义的参数类型，第二个参数：为参数名称。

#### 常量表达式

在表达式树中使用ConstantExpression表达式表示具有常量值的表达式。，看一个例子，我们定义一个int类型的常量5.并将该值赋值给上面定义的变量i

​     `// ParameterExpression表示命名的参数表达式。`
​      ` ParameterExpression i = Expression.Parameter(typeof(int), "i");`
​     `//ParameterExpression j = Expression.Variable(typeof(int), "j");`
​     `ConstantExpression constExpr = Expression.Constant(5, typeof(int));`
​     `// 创建一个表示赋值运算的 System.Linq.Expressions.BinaryExpression`
​       `//表示包含二元运算符的表达式。
​      BinaryExpression binaryExpression = Expression.Assign(i, constExpr);`

#### Lambda表达式树

`Expression<Func<T, bool>> where=......`

#### Tips

- Func为是传委托 例如<T,bool> 为传入T类型返回值类型为bool的委托