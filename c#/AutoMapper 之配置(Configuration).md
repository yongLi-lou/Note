# **AutoMapper 之配置(Configuration)**

# 配置(Configuration)

通过构造函数创建并初始化`MapperConfiguration`实例：

```c#
config = new MapperConfiguration(cfg => {
    cfg.CreateMap<Foo, Bar>();
    cfg.AddProfile<FooProfile>();
});
```

`MapperConfiguration`可以静态存储在静态字段或者依赖注入容器中。一经创建就无法更改/修改。

或者，您可以使用静态Mapper实例初始化AutoMapper：

```c#
Mapper.Initialize(cfg => {
    cfg.CreateMap<Foo, Bar>();
    cfg.AddProfile<FooProfile>();
});
```

## 配置文件实例

使用配置文件来组织你的映射配置是一个很好的方式。创建继承自Profile的类并把配置写在构造函数中：

```c#
// 这种方式从5.0版本开始public class OrganizationProfile : Profile
{    public OrganizationProfile()
    {
        CreateMap<Foo, FooDto>();        // 在这里使用 CreateMap... 等等 (Profile 方法跟 Configuration方法一致)
    }
}// 4.x到5.0版本，使用以下方式，不过这已经过时了：// public class OrganizationProfile : Profile// {//     protected override void Configure()//     {//         CreateMap<Foo, FooDto>();//     }// }
```

在早期版本中`Configure`方法用来代替构造函数。在5.0版本中，`Configure()` 已经过时并在6.0版本中移除。

Configuration 内部的配置文件仅适用于配置文件内部的映射。Configuration 应用于根配置，则适用于所有被创建的映射。

## 自动配置之程序集扫描

配置文件有多种方式可以直接添加到主映射配置中：

```c#
cfg.AddProfile<OrganizationProfile>();cfg.AddProfile(new OrganizationProfile());
```

or by automatically scanning for profiles:

```c#
// 在程序集中扫描所有配置//使用实例的方式：
var config = new MapperConfiguration(cfg => {
    cfg.AddProfiles(myAssembly);
});//使用静态的方式：
Mapper.Initialize(cfg => cfg.AddProfiles(myAssembly));//也可以使用程序集名称：
Mapper.Initialize(cfg =>
    cfg.AddProfiles(new [] {        "Foo.UI",        "Foo.Core"
    });
);// 还可以使用程序集类型：
Mapper.Initialize(cfg =>
    cfg.AddProfiles(new [] {        typeof(HomeController),        typeof(Entity)
    });
);
```

Automapper将扫描指定的程序集，将继承自Profile的类添加到配置中。

## 命名约定

你可以设置源和目标的命名约定

```c#
Mapper.Initialize(cfg => {
  cfg.SourceMemberNamingConvention = new LowerUnderscoreNamingConvention();
  cfg.DestinationMemberNamingConvention = new PascalCaseNamingConvention();
});
```

以下属性将相互映射：`property_name - > PropertyName`。

你也可以在每个配置文件级别设置命名约定。

```c#
public class OrganizationProfile : Profile
{  public OrganizationProfile()
  {
    SourceMemberNamingConvention = new LowerUnderscoreNamingConvention();
    DestinationMemberNamingConvention = new PascalCaseNamingConvention();    //将CreateMap 等等放在这里
  }
}
```

## 字符替换

你也可以在成员名字匹配期间替换源成员的单个字符或单词：

```c#
public class Source{
    public int Value { get; set; }    public int Ävíator { get; set; }    public int SubAirlinaFlight { get; set; }
}public class Destination{
    public int Value { get; set; }    public int Aviator { get; set; }    public int SubAirlineFlight { get; set; }
}
```

替换一个字符或者转换一个单词：

```c#
Mapper.Initialize(c =>{
    c.ReplaceMemberName("Ä", "A");
    c.ReplaceMemberName("í", "i");
    c.ReplaceMemberName("Airlina", "Airline");
});
```

## 识别前/后缀

某些时候你的源/目标成员有公共的前/后缀这使得因为名称不匹配导致你需要定义一堆自定义成员映射。可以使用识别前/后缀来解决这个问题：

```c#
public class Source {
    public int frmValue { get; set; }    public int frmValue2 { get; set; }
}public class Dest {
    public int Value { get; set; }    public int Value2 { get; set; }
}
Mapper.Initialize(cfg => {
    cfg.RecognizePrefixes("frm");
    cfg.CreateMap<Source, Dest>();
});
Mapper.AssertConfigurationIsValid();
```

AutoMapper 默认识别"Get"前缀，如果你需要清除该前缀：

```c#
Mapper.Initialize(cfg => {
    cfg.ClearPrefixes();
    cfg.RecognizePrefixes("tmp");
});
```

## 全局属性/字段过滤

AutoMapper默认尝试映射所有的公共属性/字段。你可以使用属性/字段过滤器来过滤掉属性/字段：

```c#
Mapper.Initialize(cfg =>{    // 不映射任何字段
    cfg.ShouldMapField = fi => false;    // 映射getter为公共或私有的属性
    cfg.ShouldMapProperty = pi =>
        pi.GetMethod != null && (pi.GetMethod.IsPublic || pi.GetMethod.IsPrivate);
});
```

## 配置可见性

AutoMapper默认只识别公共成员。虽然也能映射私有setters，但是会跳过整个属性为internal/private中internal/private的方法和属性。为了教会AutoMapper识别其它可见级别的成员，覆盖默认过滤器ShouldMapField、ShouldMapProperty：

```c#
Mapper.Initialize(cfg =>{    // 映射getter 可见级别为public 或者internal 的属性
    cfg.ShouldMapProperty = p => p.GetMethod.IsPublic || p.GetMethod.IsAssembly;
    cfg.CreateMap<Source, Destination>();
});
```

Map 配置现在将识别 internal/private 成员。

## Configuration 编译

因为表达式编译可能会占用大量资源，所以AutoMapper延迟编译类型映射，并计划在第一次执行映射的时候编译。但是，这种行为并不能总让人满意，所以你也可以告诉AutoMapper直接编译映射：

```c#
Mapper.Initialize(cfg => {});
Mapper.Configuration.CompileMappings();
```

对于几百个映射，这可能需要几秒钟。

## 重置静态映射配置

静态Mapper.Initialize意味着只被调用一次。重置的静态映射配置（例如，在测试开始时）：

```c#
Mapper.Reset();

Mapper.Initialize(cfg => { /* 重新配置 */ });
```

不应在生产代码中使用重置。它的意义在于支持测试场景。