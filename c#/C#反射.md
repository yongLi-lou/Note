# C#反射

获取类型的成员

Type 类的 GetMembers 方法用来获取该类型的所有成员，包括方法和属性，可通过 BindingFlags 标志来筛选这些成员。

```c#
var members =typeof(object).GetMembers(BindingFlags.Public | BindingFlags.Static |BindingFlags.Instance);
foreach (var member in members)
            {
                Console.WriteLine($"{member.Name} is a {member.MemberType}");

            }
```

输出：
GetType is a Method GetHashCode is a Method ToString is a Method Equals is a
Method ReferenceEquals is a Method .ctor is a Constructor
GetMembers 方法也可以不传 BindingFlags，默认返回的是所有公开的成员。

对于非静态方法，需要传递对应的实例作为参数，示例：

```c#
public class Test {
    public int A { get; set; }
    public string B { get; set; }
        public void C() {
            Console.WriteLine("this is c");
        }
    }
var method = typeof(Test).GetMethod("C");
Test test = new Test();
            Console.WriteLine(method.Name);
            method.Invoke(test,null );
```

*参数列表为空时和静态方法时invoke（）参数本别传null*

如果是泛型方法，则还需要通过泛型参数来创建泛型方法，示例：

```c#
class Program { public static void Main() { 
    // 反射调用泛型方法 
    MethodInfo method1 =typeof(Sample).GetMethod("GenericMethod"); 
    MethodInfo generic1 =method1.MakeGenericMethod(typeof(string)); generic1.Invoke(sample, null); //反射调用静态泛型方法 
    MethodInfo method2 = typeof(Sample).GetMethod("StaticMethod");
MethodInfo generic2 = method2.MakeGenericMethod(typeof(string));
generic2.Invoke(null, null); } } 
public class Sample { 
    public void GenericMethod<T>() { //... } public static void StaticMethod<T>() { //... } 
}
```

使用 Activator 类

使用 Activator 类动态创建一个类的实例是最常见的做法，示例：

```c#
Type type = typeof(BigInteger);
            object result =Activator.CreateInstance(type);
            Console.WriteLine(result); // 输出：0 
            object result2 = Activator.CreateInstance(type,123);
            Console.WriteLine(result2); // 输出：123
```

动态创建泛类型实例，需要先创建开放泛型（如List<>），再根据泛型参数转换为具象泛型（如List<string>），示例：

动态创建泛类型实例，需要先创建开放泛型（如List<>），再根据泛型参数转换为具象泛型（如List<string>），示例：
// 先创建开放泛型 Type openType = typeof(List<>); // 再创建具象泛型 Type[] tArgs = {
typeof(string) }; Type target = openType.MakeGenericType(tArgs); // 最后创建泛型实例
List<string> result = (List<string>)Activator.CreateInstance(target);
如果你不知道什么是开放泛型和具象泛型，请看本文最后一节。

使用构造器反射

也可以通过反射构造器的方式动态创建类的实例，比上面使用 Activator 类要稍稍麻烦些，但性能要好些。示例：
ConstructorInfo c = typeof(T).GetConstructor(new[] { typeof(string) }); if (c
== null) throw new InvalidOperationException("..."); T instance =
(T)c.Invoke(new object[] { "test" });
使用 FormatterServices 类

如果你想创建某个类的实例的时候不执行构造函数和属性初始化，可以使用 FormatterServices 的 GetUninitializedObject
方法。示例：
class Program { static void Main() { MyClass instance =
(MyClass)FormatterServices.GetUninitializedObject(typeof(MyClass));
Console.WriteLine(instance.MyProperty1); // 输出：0
Console.WriteLine(instance.MyProperty2); // 输出：0 } } public class MyClass {
public MyClass(int val) { MyProperty1 = val < 1 ? 1 : val; } public int
MyProperty1 { get; } public int MyProperty2 { get; set; } = 2; }
获取属性或方法的强类型委托


通过反射获取到对象的属性和方法后，如果你想通过强类型的方法来访问或调用，可以在中间加一层委托。这样的好处是有利于封装，调用者可以明确的知道调用时需要传什么参数。
比如下面这个方法，把 Math.Max 方法提取为一个强类型委托：
var tArgs = new Type[] { typeof(int), typeof(int) }; var maxMethod =
typeof(Math).GetMethod("Max", tArgs); var strongTypeDelegate = (Func<int, int,
int>)Delegate .CreateDelegate(typeof(Func<int, int, int>), null, maxMethod);
Console.WriteLine("3 和 5 之间最大的是：{0}", strongTypeDelegate(3, 5)); // 输出：5
这个技巧也适用于属性，可以获取强类型的 Getter 和 Setter。示例：
var theProperty = typeof(MyClass).GetProperty("MyIntProperty"); // 强类型 Getter
var theGetter = theProperty.GetGetMethod(); var strongTypeGetter =
(Func<MyClass, int>)Delegate .CreateDelegate(typeof(Func<MyClass, int>),
theGetter); var intVal = strongTypeGetter(target); // 相关于：target.MyIntProperty
// 强类型 Setter var theSetter = theProperty.GetSetMethod(); var strongTypeSetter
= (Action<MyClass, int>)Delegate .CreateDelegate(typeof(Action<MyClass, int>),
theSetter); strongTypeSetter(target, 5); // 相当于：target.MyIntProperty = 5
反射获取自定义特性

以下是四个常见的场景示例。

示例一，找出一个类中标注了某个自定义特性（比如 MyAtrribute）的属性。
var props = type .GetProperties(BindingFlags.NonPublic | BindingFlags.Public |
BindingFlags.Instance) .Where(prop =>Attribute.IsDefined(prop,
typeof(MyAttribute)));
示例二，找出某个属性的所有自定义特性。
var attributes = typeof(t).GetProperty("Name").GetCustomAttributes(false);
示例三，找出程序集所有标注了某个自定义特性的类。
static IEnumerable<Type> GetTypesWithAttribute(Assembly assembly) {
foreach(Type type inassembly.GetTypes()) { if
(type.GetCustomAttributes(typeof(MyAttribute), true).Length > 0) { yield return
type; } } }
示例四，在运行时读取自定义特性的值
public static class AttributeExtensions { public static TValue
GetAttribute<TAttribute, TValue>( this Type type, string MemberName,
Func<TAttribute, TValue> valueSelector, bool inherit = false) where TAttribute
: Attribute { var att = type.GetMember(MemberName).FirstOrDefault()
.GetCustomAttributes(typeof(TAttribute), inherit) .FirstOrDefault() as
TAttribute; if (att != null) { return valueSelector(att); } return default; } }
// 使用： class Program { static void Main() { // 读取 MyClass 类的 MyMethod 方法的
Description 特性的值 var description = typeof(MyClass) .GetAttribute("MyMethod",
(DescriptionAttribute d) => d.Description); Console.WriteLine(description); //
输出：Hello } } public class MyClass { [Description("Hello")] public void
MyMethod() { } }
动态实例化接口的所有实现类（插件激活）

通过反射来动态实例化某个接口的所有实现类，常用于实现系统的插件式开发。比如在程序启动的时候去读取指定文件夹（如 Plugins）中的 dll
文件，通过反射获取 dll 中所有实现了某个接口的类，并在适当的时候将其实例化。大致实现如下：
interface IPlugin { string Description { get; } void DoWork(); }
某个在独立 dll 中的类：
class HelloPlugin : IPlugin { public string Description => "A plugin that says
Hello"; public void DoWork() { Console.WriteLine("Hello"); } }
在你的系统启动的时候动态加载该 dll，读取实现了 IPlugin 接口的所有类的信息，并将其实例化。
public IEnumerable<IPlugin> InstantiatePlugins(string directory) { var
assemblyNames = Directory.GetFiles(directory, "*.addin.dll") .Select(name =>
new FileInfo(name).FullName).ToArray(); foreach (var fileName assemblyNames)
AppDomain.CurrentDomain.Load(File.ReadAllBytes(fileName)); var assemblies =
assemblyNames.Select(System.Reflection.Assembly.LoadFile); var typesInAssembly
= assemblies.SelectMany(asm =>asm.GetTypes()); var pluginTypes =
typesInAssembly.Where(type => typeof (IPlugin).IsAssignableFrom(type)); return
pluginTypes.Select(Activator.CreateInstance).Cast<IPlugin>(); }
检查泛型实例的泛型参数

前文提到了构造泛型和具象泛型，这里解释一下。大多时候我们所说的泛型都是指构造泛型，有时候也被称为具象泛型。比如 List<int>
就是一个构造泛型，因为它可以通过 new 来实例化。相应的，List<> 泛型是非构造泛型，有时候也被称为开放泛型
，它不能被实例化。开放泛型通过反射可以转换为任意的具象泛型，这一点前文有示例。

假如现在有一个泛型实例，出于某种需求，我们想知道构建这个泛型实例需要用什么泛型参数。比如某人创建了一个 List<T>
泛型的实例，并把它作为参数传给了我们的一个方法：
var myList = newList<int>(); ShowGenericArguments(myList);
我们的方法签名是这样的：
public void ShowGenericArguments(object o)
这时，作为此方法的编写者，我们并不知道这个 o
对象具体是用什么类型的泛型参数构建的。通过反射，我们可以得到泛型实例的很多信息，其中最简单的就是判断一个类型是不是泛型：
public void ShowGenericArguments(object o) { if (o == null) return; Type t
=o.GetType(); if (!t.IsGenericType) return; ... }
由于 List<> 本身也是泛型，所以上面的判断不严谨，我们需要知道的是对象是不是一个构造泛型（List<int>）。而 Type 类还提供了一些有用的属性：
typeof(List<>).IsGenericType // true typeof(List<>).IsGenericTypeDefinition //
true typeof(List<>).IsConstructedGenericType// false
typeof(List<int>).IsGenericType // true
typeof(List<int>).IsGenericTypeDefinition // false
typeof(List<int>).IsConstructedGenericType// true
IsConstructedGenericType 和 IsGenericTypeDefinition 分别用来判断某个泛型是不是构造泛型和非构造泛型。

再结合 Type 的 GetGenericArguments() 方法，就可以很容易地知道某个泛型实例是用什么泛型参数构建的了，例如：
static void ShowGenericArguments(object o) { if (o == null) return; Type t =
o.GetType(); if (!t.IsConstructedGenericType) return; foreach (Type
genericTypeArgument in t.GetGenericArguments())
Console.WriteLine(genericTypeArgument.Name); }
以上是关于反射的干货知识，都是从实际项目开发中总结而来，希望对你的开发有帮助。

