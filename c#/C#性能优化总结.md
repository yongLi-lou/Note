# C#性能优化总结

**1. C#语言方面**

**1.1 垃圾回收**
垃圾回收解放了手工管理对象的工作，提高了程序的健壮性，但副作用就是程序代码可能对于对象创建变得随意。
**1.1.1 避免不必要的对象创建
**由于垃圾回收的代价较高，所以C#程序开发要遵循的一个基本原则就是避免不必要的对象创建。以下列举一些常见的情形。
**1.1.1.1 避免循环创建对象 ★
**如果对象并不会随每次循环而改变状态，那么在循环中反复创建对象将带来性能损耗。高效的做法是将对象提到循环外面创建。
**1.1.1.2 在需要逻辑分支中创建对象
**如果对象只在某些逻辑分支中才被用到，那么应只在该逻辑分支中创建对象。
**1.1.1.3 使用常量避免创建对象**
程序中不应出现如 new Decimal(0) 之类的代码，这会导致小对象频繁创建及回收，正确的做法是使用Decimal.Zero常量。我们有设计自己的类时，也可以学习这个设计手法，应用到类似的场景中。
**1.1.1.4 使用StringBuilder做字符串连接

**1.1.2 不要使用空析构函数 ★**
如果类包含析构函数，由创建对象时会在 Finalize 队列中添加对象的引用，以保证当对象无法可达时，仍然可以调用到 Finalize 方法。垃圾回收器在运行期间，会启动一个低优先级的线程处理该队列。相比之下，没有析构函数的对象就没有这些消耗。如果析构函数为空，这个消耗就毫无意 义，只会导致性能降低！因此，不要使用空的析构函数。
在实际情况中，许多曾在析构函数中包含处理代码，但后来因为种种原因被注释掉或者删除掉了，只留下一个空壳，此时应注意把析构函数本身注释掉或删除掉。
**1.1.3 实现 IDisposable 接口
**垃圾回收事实上只支持托管内在的回收，对于其他的非托管资源，例如 Window GDI 句柄或数据库连接，在析构函数中释放这些资源有很大问题。原因是垃圾回收依赖于内在紧张的情况，虽然数据库连接可能已濒临耗尽，但如果内存还很充足的话， 垃圾回收是不会运行的。
C#的 IDisposable 接口是一种显式释放资源的机制。通过提供 using 语句，还简化了使用方式（编译器自动生成 try ... finally 块，并在 finally 块中调用 Dispose 方法）。对于申请非托管资源对象，应为其实现 IDisposable 接口，以保证资源一旦超出 using 语句范围，即得到及时释放。这对于构造健壮且性能优良的程序非常有意义！
为防止对象的 Dispose 方法不被调用的情况发生，一般还要提供析构函数，两者调用一个处理资源释放的公共方法。同时，Dispose 方法应调用 System.GC.SuppressFinalize(this)，告诉垃圾回收器无需再处理 Finalize 方法了。
**1.2 String 操作**
**1.2.1 使用 StringBuilder 做字符串连接**
String 是不变类，使用 + 操作连接字符串将会导致创建一个新的字符串。如果字符串连接次数不是固定的，例如在一个循环中，则应该使用 StringBuilder 类来做字符串连接工作。因为 StringBuilder 内部有一个 StringBuffer ，连接操作不会每次分配新的字符串空间。只有当连接后的字符串超出 Buffer 大小时，才会申请新的 Buffer 空间。典型代码如下：

```c#
 StringBuilder sb = new StringBuilder( 256 );
 for ( int i = 0 ; i < Results.Count; i ++ )
 {
 sb.Append (Results[i]);
 }
```

如果连接次数是固定的并且只有几次，此时应该直接用 + 号连接，保持程序简洁易读。实际上，编译器已经做了优化，会依据加号次数调用不同参数个数的 String.Concat 方法。例如：

```c#
String str = str1 + str2 + str3 + str4;
```

会被编译为 String.Concat(str1, str2, str3, str4)。该方法内部会计算总的 String 长度，仅分配一次，并不会如通常想象的那样分配三次。作为一个经验值，当字符串连接操作达到 10 次以上时，则应该使用 StringBuilder。
这里有一个细节应注意：StringBuilder 内部 Buffer 的缺省值为 16 ，这个值实在太小。按 StringBuilder 的使用场景，Buffer 肯定得重新分配。经验值一般用 256 作为 Buffer 的初值。当然，如果能计算出最终生成字符串长度的话，则应该按这个值来设定 Buffer 的初值。使用 new StringBuilder(256) 就将 Buffer 的初始长度设为了256。

**1.2.2 避免不必要的调用 ToUpper 或 ToLower 方法**
String是不变类，调用ToUpper或ToLower方法都会导致创建一个新的字符串。如果被频繁调用，将导致频繁创建字符串对象。这违背了前面讲到的“避免频繁创建对象”这一基本原则。
例如，bool.Parse方法本身已经是忽略大小写的，调用时不要调用ToLower方法。
另一个非常普遍的场景是字符串比较。高效的做法是使用 Compare 方法，这个方法可以做大小写忽略的比较，并且不会创建新字符串。
还有一种情况是使用 HashTable 的时候，有时候无法保证传递 key 的大小写是否符合预期，往往会把 key 强制转换到大写或小写方法。实际上 HashTable 有不同的构造形式，完全支持采用忽略大小写的 key: new HashTable(StringComparer.OrdinalIgnoreCase)。
**1.2.3 最快的空串比较方法**
将String对象的Length属性与0比较是最快的方法：if (str.Length == 0)
其次是与String.Empty常量或空串比较：if (str == String.Empty)或if (str == "")
注：C#在编译时会将程序集中声明的所有字符串常量放到保留池中（intern pool），相同常量不会重复分配。
**1.3 多线程
1.3.1 线程同步**
线 程同步是编写多线程程序需要首先考虑问题。C#为同步提供了 Monitor、Mutex、AutoResetEvent 和 ManualResetEvent 对象来分别包装 Win32 的临界区、互斥对象和事件对象这几种基础的同步机制。C#还提供了一个lock语句，方便使用，编译器会自动生成适当的 Monitor.Enter 和 Monitor.Exit 调用。

**1.3.1.1 同步粒度**

同步粒度可以是整个方法，也可以是方法中某一段代码。为方法指定 MethodImplOptions.Synchronized 属性将标记对整个方法同步。例如：

```c#
[MethodImpl(MethodImplOptions.Synchronized)]
 public static SerialManager GetInstance()
{
 if (instance == null )
{
 instance = new SerialManager();
}
 return instance;
 }
```

通常情况下，应减小同步的范围，使系统获得更好的性能。简单将整个方法标记为同步不是一个好主意，除非能确定方法中的每个代码都需要受同步保护。

**1.3.1.2 同步策略**
使用 lock 进行同步，同步对象可以选择 Type、this 或为同步目的专门构造的成员变量。
**避免锁定Type★**
锁定Type对象会影响同一进程中所有AppDomain该类型的所有实例，这不仅可能导致严重的性能问题，还可能导致一些无法预期的行为。这是一个很不 好的习惯。即便对于一个只包含static方法的类型，也应额外构造一个static的成员变量，让此成员变量作为锁定对象。
避免锁定 this
锁定 this 会影响该实例的所有方法。假设对象 obj 有 A 和 B 两个方法，其中 A 方法使用 lock(this) 对方法中的某段代码设置同步保护。现在，因为某种原因，B 方法也开始使用 lock(this) 来设置同步保护了，并且可能为了完全不同的目的。这样，A 方法就被干扰了，其行为可能无法预知。所以，作为一种良好的习惯，建议避免使用 lock(this) 这种方式。
**使用为同步目的专门构造的成员变量**
这是推荐的做法。方式就是 new 一个 object 对象， 该对象仅仅用于同步目的。
如果有多个方法都需要同步，并且有不同的目的，那么就可以为些分别建立几个同步成员变量。

**1.3.1.4 集合同步**
C#为各种集合类型提供了两种方便的同步机制：Synchronized 包装器和 SyncRoot 属性。

```c#
// Creates and initializes a new ArrayList
 ArrayList myAL = new ArrayList();
myAL.Add( " The " );
myAL.Add( " quick " );
myAL.Add( " brown " );
myAL.Add( " fox " );
// Creates a synchronized wrapper around the ArrayList
clip_image001[13] ArrayList mySyncdAL = ArrayList.Synchronized(myAL);
```

调用 Synchronized 方法会返回一个可保证所有操作都是线程安全的相同集合对象。考虑 mySyncdAL[0] = mySyncdAL[0] + "test" 这一语句，读和写一共要用到两个锁。一般讲，效率不高。推荐使用 SyncRoot 属性，可以做比较精细的控制。

**1.3.2 使用 ThreadStatic 替代 NameDataSlot ★**
存 取 NameDataSlot 的 Thread.GetData 和 Thread.SetData 方法需要线程同步，涉及两个锁：一个是 LocalDataStore.SetData 方法需要在 AppDomain 一级加锁，另一个是 ThreadNative.GetDomainLocalStore 方法需要在 Process 一级加锁。如果一些底层的基础服务使用了 NameDataSlot，将导致系统出现严重的伸缩性问题。
规避这个问题的方法是使用 ThreadStatic 变量。示例如下：

```c#
public sealed class InvokeContext
 {
 [ThreadStatic]
 private static InvokeContext current;
 private Hashtable maps = new Hashtable();
}
```

**1.3.3 多线程编程技巧
1.3.3.1 使用 Double Check 技术创建对象**

```c#
 internal IDictionary KeyTable
 {
 get
 {
 if ( this ._keyTable == null )
 {
 lock ( base ._lock)
 {
 if ( this ._keyTable == null )
 {
 this ._keyTable = new Hashtable();
 }
 }
 }
 return this ._keyTable;
 }
}
```

创建单例对象是很常见的一种编程情况。一般在 lock 语句后就会直接创建对象了，但这不够安全。因为在 lock 锁定对象之前，可能已经有多个线程进入到了第一个 if 语句中。如果不加第二个 if 语句，则单例对象会被重复创建，新的实例替代掉旧的实例。如果单例对象中已有数据不允许被破坏或者别的什么原因，则应考虑使用 Double Check 技术。
**1.4 类型系统
1.4.1 避免无意义的变量初始化动作**
CLR保证所有对象在访问前已初始化，其做法是将分配的内存清零。因此，不需要将变量重新初始化为0、false或null。
需要注意的是：方法中的局部变量不是从堆而是从栈上分配，所以C#不会做清零工作。如果使用了未赋值的局部变量，编译期间即会报警。不要因为有这个印象而对所有类的成员变量也做赋值动作，两者的机理完全不同！
**1.4.2 ValueType 和 ReferenceType
1.4.2.1 以引用方式传递值类型参数**
值类型从调用栈分配，引用类型从托管堆分配。当值类型用作方法参数时，默认会进行参数值复制，这抵消了值类型分配效率上的优势。作为一项基本技巧，以引用方式传递值类型参数可以提高性能。
**1.4.2.2 为 ValueType 提供 Equals 方法**
.net 默认实现的 ValueType.Equals 方法使用了反射技术，依靠反射来获得所有成员变量值做比较，这个效率极低。如果我们编写的值对象其 Equals 方法要被用到（例如将值对象放到 HashTable 中），那么就应该重载 Equals 方法。

```c#
public struct Rectangle
 {
 public double Length;
 public double Breadth;
 public override bool Equals ( object ob)
 {
 if (ob is Rectangle)
 return Equels ((Rectangle)ob))
 else
 return false ;
 }
 private bool Equals (Rectangle rect)
 {
 return this .Length == rect.Length && this .Breadth == rect.Breach;
 }
}
```

**1.4.2.3 避免装箱和拆箱**
C#可以在值类型和引用类型之间自动转换，方法是装箱和拆箱。装箱需要从堆上分配对象并拷贝值，有一定性能消耗。如果这一过程发生在循环中或是作为底层方法被频繁调用，则应该警惕累计的效应。
一种经常的情形出现在使用集合类型时。例如：

```c#
ArrayList al = new ArrayList();
 for ( int i = 0 ; i < 1000 ; i ++ )
{
 al.Add(i); // Implicitly boxed because Add() takes an object
}
 int f = ( int )al[ 0 ]; // The element is unboxed
```

**1.5 异常处理**
异常也是现代语言的典型特征。与传统检查错误码的方式相比，异常是强制性的（不依赖于是否忘记了编写检查错误码的代码）、强类型的、并带有丰富的异常信息（例如调用栈）。
**1.5.1 不要吃掉异常★**
关于异常处理的最重要原则就是：不要吃掉异常。这个问题与性能无关，但对于编写健壮和易于排错的程序非常重要。这个原则换一种说法，就是不要捕获那些你不能处理的异常。
吃掉异常是极不好的习惯，因为你消除了解决问题的线索。一旦出现错误，定位问题将非常困难。除了这种完全吃掉异常的方式外，只将异常信息写入日志文件但并不做更多处理的做法也同样不妥。
**1.5.2 不要吃掉异常信息★**
有些代码虽然抛出了异常，但却把异常信息吃掉了。
为异常披露详尽的信息是程序员的职责所在。如果不能在保留原始异常信息含义的前提下附加更丰富和更人性化的内容，那么让原始的异常信息直接展示也要强得多。千万不要吃掉异常。
**1.5.3 避免不必要的抛出异常**
抛出异常和捕获异常属于消耗比较大的操作，在可能的情况下，应通过完善程序逻辑避免抛出不必要不必要的异常。与此相关的一个倾向是利用异常来控制处理逻辑。尽管对于极少数的情况，这可能获得更为优雅的解决方案，但通常而言应该避免。
**1.5.4 避免不必要的重新抛出异常**
如果是为了包装异常的目的（即加入更多信息后包装成新异常），那么是合理的。但是有不少代码，捕获异常没有做任何处理就再次抛出，这将无谓地增加一次捕获异常和抛出异常的消耗，对性能有伤害。
**1.6 反射**
反射是一项很基础的技术，它将编译期间的静态绑定转换为延迟到运行期间的动态绑定。在很多场景下（特别是类框架的设计），可以获得灵活易于扩展的架构。但带来的问题是与静态绑定相比，动态绑定会对性能造成较大的伤害。
**1.6.1 反射分类**
type comparison ：类型判断，主要包括 is 和 typeof 两个操作符及对象实例上的 GetType 调用。这是最轻型的消耗，可以无需考虑优化问题。注意 typeof 运算符比对象实例上的 GetType 方法要快，只要可能则优先使用 typeof 运算符。
member enumeration ： 成员枚举，用于访问反射相关的元数据信息，例如Assembly.GetModule、Module.GetType、Type对象上的 IsInterface、IsPublic、GetMethod、GetMethods、GetProperty、GetProperties、 GetConstructor调用等。尽管元数据都会被CLR缓存，但部分方法的调用消耗仍非常大，不过这类方法调用频度不会很高，所以总体看性能损失程 度中等。
member invocation：成员调用，包括动态创建对象及动态调用对象方法，主要有Activator.CreateInstance、Type.InvokeMember等。
**1.6.2 动态创建对象**
C#主要支持 5 种动态创建对象的方式：
\1. Type.InvokeMember
\2. ContructorInfo.Invoke
\3. Activator.CreateInstance(Type)
\4. Activator.CreateInstance(assemblyName, typeName)
\5. Assembly.CreateInstance(typeName)
最快的是方式 3 ，与 Direct Create 的差异在一个数量级之内，约慢 7 倍的水平。其他方式，至少在 40 倍以上，最慢的是方式 4 ，要慢三个数量级。

**1.6.3 动态方法调用**
方法调用分为编译期的早期绑定和运行期的动态绑定两种，称为Early-Bound Invocation和Late-Bound Invocation。Early-Bound Invocation可细分为Direct-call、Interface-call和Delegate-call。Late-Bound Invocation主要有Type.InvokeMember和MethodBase.Invoke，还可以通过使用LCG（Lightweight Code Generation）技术生成IL代码来实现动态调用。
从测试结果看，相比Direct Call，Type.InvokeMember要接近慢三个数量级；MethodBase.Invoke虽然比Type.InvokeMember要快三 倍，但比Direct Call仍慢270倍左右。可见动态方法调用的性能是非常低下的。我们的建议是：除非要满足特定的需求，否则不要使用！
**1.6.4 推荐的使用原则
模式
**1． 如果可能，则避免使用反射和动态绑定
2． 使用接口调用方式将动态绑定改造为早期绑定
3． 使用Activator.CreateInstance(Type)方式动态创建对象
4． 使用typeof操作符代替GetType调用
**反模式
**1． 在已获得Type的情况下，却使用Assembly.CreateInstance(type.FullName)
**1.7 基本代码技巧**
这里描述一些应用场景下，可以提高性能的基本代码技巧。对处于关键路径的代码，进行这类的优化还是很有意义的。普通代码可以不做要求，但养成一种好的习惯也是有意义的。
**1.7.1 循环写法**
可以把循环的判断条件用局部变量记录下来。局部变量往往被编译器优化为直接使用寄存器，相对于普通从堆或栈中分配的变量速度快。如果访问的是复杂计算属性 的话，提升效果将更明显。for (int i = 0, j = collection.GetIndexOf(item); i < j; i++)
需要说明的是：这种写法对于CLR集合类的Count属性没有意义，原因是编译器已经按这种方式做了特别的优化。
**1.7.2 拼装字符串**
拼装好之后再删除是很低效的写法。有些方法其循环长度在大部分情况下为1，这种写法的低效就更为明显了：

```c#
public static string ToString(MetadataKey entityKey)
{
 string str = "" ;
 object [] vals = entityKey.values;
 for ( int i = 0 ; i < vals.Length; i ++ )
{
 str += " , " + vals[i].ToString();
}
 return str == "" ? "" : str.Remove( 0 , 1 );
}
```

推荐下面的写法：

```c#
if (str.Length == 0 )
 str = vals[i].ToString();
else
 str += " , " + vals[i].ToString();
```

其实这种写法非常自然，而且效率很高，完全不需要用个Remove方法绕来绕去。

**1.7.3 避免两次检索集合元素**
获取集合元素时，有时需要检查元素是否存在。通常的做法是先调用ContainsKey（或Contains）方法，然后再获取集合元素。这种写法非常符合逻辑。
但如果考虑效率，可以先直接获取对象，然后判断对象是否为null来确定元素是否存在。对于Hashtable，这可以节省一次GetHashCode调用和n次Equals比较。

如下面的示例：

```c#
 public IData GetItemByID(Guid id)
{
 IData data1 = null ;
 if ( this .idTable.ContainsKey(id.ToString())
{
 data1 = this .idTable[id.ToString()] as IData;
}
 return data1;
     }
```

其实完全可用一行代码完成：return this.idTable[id] as IData;
**1.7.4 避免两次类型转换**
考虑如下示例，其中包含了两处类型转换：

```c#
if (obj is SomeType)
{
 SomeType st = (SomeType)obj;
 st.SomeTypeMethod();
}
```

效率更高的做法如下：

```c#
SomeType st = obj as SomeType;
 if (st != null )
{
 st.SomeTypeMethod();
 }
```

**1.8 Hashtable
**Hashtable是一种使用非常频繁的基础集合类型。需要理解影响Hashtable的效率有两个因素：一是散列码（GetHashCode方法），二 是等值比较（Equals方法）。Hashtable首先使用键的散列码将对象分布到不同的存储桶中，随后在该特定的存储桶中使用键的Equals方法进 行查找。
良好的散列码是第一位的因素，最理想的情况是每个不同的键都有不同的散列码。Equals方法也很重要，因为散列只需要做一次，而存储桶中查找键可能需要做多次。从实际经验看，使用Hashtable时，Equals方法的消耗一般会占到一半以上。

System.Object类提供了默认的GetHashCode实现，使用对象在内存中的地址作为散列码。我们遇到过一个用Hashtable来缓存对 象的例子，每次根据传递的OQL表达式构造出一个ExpressionList对象，再调用QueryCompiler的方法编译得到 CompiledQuery对象。以ExpressionList对象和CompiledQuery对象作为键值对存储到Hashtable中。 ExpressionList对象没有重载GetHashCode实现，其超类ArrayList也没有，这样最后用的就是System.Object类 的GetHashCode实现。由于ExpressionList对象会每次构造，因此它的HashCode每次都不同，所以这个 CompiledQueryCache根本就没有起到预想的作用。这个小小的疏漏带来了重大的性能问题，由于解析OQL表达式频繁发生，导致 CompiledQueryCache不断增长，造成服务器内存泄漏！解决这个问题的最简单方法就是提供一个常量实现，例如让散列码为常量0。虽然这会导 致所有对象汇聚到同一个存储桶中，效率不高，但至少可以解决掉内存泄漏问题。当然，最终还是会实现一个高效的GetHashCode方法的。
以上介绍这些Hashtable机理，主要是希望大家理解：如果使用Hashtable，你应该检查一下对象是否提供了适当的GetHashCode和Equals方法实现。否则，有可能出现效率不高或者与预期行为不符的情况。