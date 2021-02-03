# new {…}和new Object(){}和new Object{}的区别

new {…}总是创建一个匿名对象，例如：

```c#
Object sample = new {};
  String sampleName = sample.GetType().Name; //<- something like "<>f__AnonymousType0" 
                                             //                   not "Object"
```

而new Object()创建一个Object类的实例

```c#
Object sample = new Object() {};
  String sampleName = sample.GetType().Name; // <- "Object"
```

因为所有对象(包括匿名对象)都是从Object派生的，所以你可以随时输入

```c#
Object sample = new {};
```

---

我用这两种方式用C#定义了类中的对象

```c#
var something = new SomeThing()
{
    Property = "SomeProperty"
}
```

```c#
var something = new SomeThing
{
    Property = "SomeProperty"
}
```

它们只在你提供参数时才有意义;你可以同时做到这一点

```c#
var something = new SomeThing("some other value")
{
    Property = "SomeProperty"
}
```

