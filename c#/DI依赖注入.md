# DI依赖注入

## 相关基本概念

### 类和对象

类是一个静态的概念，类本身不携带任何数据。当没有为类创建任何对象时，类本身不存在于内存空间中。

- new 实例化的过程

  声明引用； 
  使用new关键字创建类的对象并对其初始化；（分配内存空间） 
  将引用指向类的对象。

### 解除依赖的思想是如何产生的？

(1)原始社会里，没有社会分工。须要斧子的人(调用者)仅仅能自己去磨一把斧子(被调用者)。相应的情形为:软件程序里的调用者自己创建被调用者。

(2)进入工业社会，工厂出现。斧子不再由普通人完毕，而在工厂里被生产出来，此时须要斧子的人(调用者)找到工厂，购买斧子，无须关心斧子的制造过程。相应软件程序的简单工厂的设计模式。

(3)进入“按需分配”社会，需要斧子的人不需要找到工厂，坐在家里发出一个简单指令:须要斧子。斧子就自然出如今他面前。依赖注入。




###  控制反转 IoC
Inversion of Control

- 23种设计模式
  6个基本原则

  1.单一职责（一个方法或一个类只做一件事，为了模块内高内聚） 
  2.迪米特法则（也叫最少知道原则，为了模块间低耦合） 
  3.里氏替换（就是继承原则，子类可以无缝替代父类。很好的符合了开闭原则） 
  4.依赖倒置（类之间的依赖通过接口实现，低耦合的同时对扩展开放） 
  5.接口隔离（即把单个复杂接口拆分为多个独立接口，与上条共同实现面向接口编程） 

  6.合成复用原则（即尽量使用合成/聚合的方式，而不是使用继承。主要为了防止继承滥用而导致的类之间耦合严重。记住只有符合继承原则时才用继承）
  ---------------------

- 高级模块不应该依赖于低级模块;
两者都应该取决于抽象
- 什么是控制反转

  你把程序里的一个写死的变量改成从配置文件里读取也是一种控制反转（由程序控制反转为由框架控制），你把这个配置改成用户UI界面的一个输入文本框由用户输入也是一种控制反转（由框架控制反转为由用户自己控制）。

	- 是一种设计原则，最早由Martin Fowler提出，因为其理论提出时间和成熟时间相对较晚，所以并没有被包含在GoF的《设计模式》中。
对依赖的控制，从自己，转到自己的调用者上

- 举例场景

	- 电扇吹风

### 依赖注入 DI
Dependency Injection

- DI 和 IoC的关系

	- 实现IoC的其中一种设计方法，
依赖的对象注入到调用者，你不应该自己创建它，而是应该通过构造函数由你的调用者给你——容器 

- 常用的框架有哪些

	- 微软 core 自带的 DI
	- Autofac：貌似目前net下用的最多吧
	- Ninject：目前好像没多少人用了
	- Unity：也是较为常见

- 三种注入方法

	- 构造函数注入

	  public class StudentService : IStudentService
	      {
	          private readonly IStudentRepository _studentRepository;
	          /// <summary>
	          /// 构造注入
	          /// </summary>
	          /// <param name="studentRepository"></param>
	          public StudentService(IStudentRepository studentRepository)
	          {
	              _studentRepository = studentRepository;
	          }
	      }

		- 遵循显式依赖原则

	- 属性注入

	  public class TeacherService : ITeacherService
	      {
	          /// <summary>
	          /// 用于属性注入
	          /// </summary>
	          public ITeacherRepository TeacherRepository { get; set; }
	  
	          public string GetTeacherName(long id)
	          {
	              return TeacherRepository?.Get(111).Name;
	          }
	      }
	  ----------------------------------------
	  
	  要使用这种属性注入，在注册该属性所属类的时候，需要使用PropertiesAutowired()方法额外标注，如下：
	  

	  builder.RegisterType<TeacherService>().PropertiesAutowired();

	- 方法注入

- 三种生命周期
	注意要保持一致

	- AddTransient

		- 每次注入或请求时都会创建转瞬即逝的服务.

	- AddScoped

		- 是按范围创建的,在Web应用程序中,每个Web请求都会创建一个新的独立服务范围.

建议使用？？

	- AddSingleton
	
		- 每个DI容器创建一个单例服务,这通常意味着它们在每个应用程序只创建一次,然后用于整个应用程序生命周期.

- 举例场景

	- 自动售货机

	  容器是一个自动售货机，组件是放在里面的在售商品，服务是商品的出售名称。
	  把商品（项目里的具象对象）放入自动售货机（容器）上架的过程叫注册；
	  注册的时候会给商品贴上标签，标注该商品的名称，这个名称就叫服务；
	  我们还可以标注这个商品的适用人群和过期时间等（生命周期作用域）；
	  把这个包装后的商品放入自动售货机后，它就变成了在售商品（组件）。
	  当有顾客需要某个商品时，他只要对着售货机报一个商品名（服务名），自动售货机找到对应商品，抛出给客户，这个抛给你的过程，就叫做注入你；
	  而且这个售货机比较智能，抛出前还可以先判断商品是不是过期了，该不该抛给你。

### 仓储的概念

- 不是为了换数据库！

## 为什么要使用依赖注入

### 修改配置文件不需要重启服务器

### 减少代码量，更灵活

### 防止重构

- 参数的变化

### 项目代码松耦合

### 忽略内部复杂依赖

### 管理生命周期

- 防止内存泄漏

### 单元测试

- Mock

### 方便进行代理

- AOP

## 依赖注入步骤

### 如何合理的暴露

### 含有接口的服务层注入

- 仓储接口注入到服务层
- 引用Autofac nuget包

  安装相应的程序包：
  
  Autofac
  Autofac.Extensions.DependencyInjection

- 新建AutoFac容器
- 初始化容器，向容器注册所有需要的依赖对象，
程序集反射注入，
- 服务接口注入到 controller
- 容器根据暴露类型解析相应的对象——controller
- 去实例化——service
- 到底要不要解耦？
- 查看是否注入成功了
- 常见的错误

	- 路径不对，dll未能反射成功
	- service 层没有写构造函数
	- service 层构造函数中没有注入 IRepo 仓储接口
	- Api 层没有引用 IRepo 仓储接口层

### 单独服务层注入

- 和有接口的基本一致，
  不用写 .AsImplementedInterfaces() 

  ////因为没有接口层，所以不能实现解耦，只能用 Load 方法。
   ////注意如果使用没有接口的服务，并想对其使用 AOP 拦截，就必须设置为虚方法
   ////var assemblysServicesNoInterfaces = Assembly.Load("Blog.Core.Services");
   ////builder.RegisterAssemblyTypes(assemblysServicesNoInterfaces);

### 单类注入

- 类必须是虚方法 virtual 

  ////只能注入该类中的虚方法
   builder.RegisterAssemblyTypes(Assembly.GetAssembly(typeof(Love)))
       .EnableClassInterceptors()
       .InterceptedBy(typeof(BlogLogAOP));

### 一个接口多个实现

- 获取最后一个实例

### 一个类多个接口

## DI 其他方面使用

### AOP

