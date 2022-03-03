# Ifreesql使用

## 安装包

FreeSql.Provider.xxx([可选的驱动](http://freesql.net/guide/install.html))

```bash
dotnet add packages FreeSql
dotnet add packages FreeSql.Provider.Sqlite
```

or

```text
Install-Package FreeSql
Install-Package FreeSql.Provider.Sqlite
```

## [#](http://freesql.net/guide/getting-started.html#声明)声明

```csharp
IFreeSql fsql = new FreeSql.FreeSqlBuilder()
    .UseConnectionString(FreeSql.DataType.Sqlite, @"Data Source=db1.db")
    .UseAutoSyncStructure(true) //自动同步实体结构到数据库，FreeSql不会扫描程序集，只有CRUD时才会生成表。
    .Build(); //请务必定义成 Singleton 单例模式
    
    //注意： IFreeSql 在项目中应以单例声明，而不是在每次使用的时候创建。
```

1
2
3
4
5
6

- .NET Core 单例 Startup.cs

```csharp
public void ConfigureServices(IServiceCollection services)
{
    IFreeSql fsql = new FreeSql.FreeSqlBuilder()
      .UseConnectionString(FreeSql.DataType.Sqlite, @"Data Source=db1.db")
      .UseAutoSyncStructure(true) //自动同步实体结构到数据库，FreeSql不会扫描程序集，只有CRUD时才会生成表。
      .Build();
    services.AddSingleton<IFreeSql>(fsql);
}
```

- [.NET Core注入多个FreeSql实例在新窗口打开](https://github.com/dotnetcore/FreeSql/issues/44)
- .NET Framework 单例

```csharp
public class DB
{
   static Lazy<IFreeSql>sqliteLazy = new Lazy<IFreeSql>(() => new FreeSql.FreeSqlBuilder()
         .UseConnectionString(FreeSql.DataType.Sqlite, @"Data Source=db1.db")
         .UseAutoSyncStructure(true) //自动同步实体结构到数据库，FreeSql不会扫描程序集，只有CRUD时才会生成表。
         .Build());

    public static IFreeSql Sqlite => sqliteLazy.Value;
}
```

`IFreeSql` 是 `ORM` 最顶级对象，所有操作都是使用它的方法或者属性：

```c
fsql.Select<T>(); //查询
fsql.Insert<T>(); //插入
fsql.Update<T>(); //更新
fsql.Delete<T>(); //删除
fsql.InsertOrUpdate<T>()// 插入或更新
fsql.Transaction(..); //事务

fsql.CodeFirst; //CodeFirst 对象
fsql.DbFirst; //DbFirst 对象
fsql.Ado; //Ado 对象
fsql.Aop; //Aop 对象
fsql.GlobalFilter; //全局过滤器对象
```

## [#](http://freesql.net/guide/getting-started.html#迁移)迁移

程序运行中`FreeSql`会检查`AutoSyncStructure`参数，以此条件判断是否对比实体与数据库结构之间的变化，达到自动迁移的目的,更多请查看[CodeFirst](http://freesql.net/guide/code-first.html)文档，

> 注意：谨慎、谨慎、谨慎在生产环境中使用该功能。

> 注意：谨慎、谨慎、谨慎在生产环境中使用该功能。

> 注意：谨慎、谨慎、谨慎在生产环境中使用该功能。

## [#](http://freesql.net/guide/getting-started.html#查询)查询

```csharp
var blogs = fsql.Select<Blog>()
    .Where(b => b.Rating > 3)
    .OrderBy(b => b.Url)
    .Skip(100)
    .Limit(10) //第100行-110行的记录
    .ToList();
```

## [#](http://freesql.net/guide/getting-started.html#插入)插入

```csharp
var blog = new Blog { Url = "http://sample.com" };
blog.BlogId = (int)fsql.Insert<Blog>()
    .AppendData(blog)
    .ExecuteIdentity();
```

## [#](http://freesql.net/guide/getting-started.html#更新)更新

```csharp
fsql.Update<Blog>()
    .Set(b => b.Url, "http://sample2222.com")
    .Where(b => b.Url == "http://sample.com")
    .ExecuteAffrows();
```

## [#](http://freesql.net/guide/getting-started.html#删除)删除

```csharp
fsql.Delete<Blog>()
    .Where(b => b.Url == "http://sample.com")
    .ExecuteAffrows();
```

# [#](http://freesql.net/guide/getting-started.html#freesqlbuilder)FreeSqlBuilder

| 方法                                  | 返回值      | 说明                                                         |
| ------------------------------------- | ----------- | ------------------------------------------------------------ |
| UseConnectionString                   | this        | 设置连接串                                                   |
| UseSlave                              | this        | 设置从数据库，支持多个                                       |
| UseConnectionFactory                  | this        | 设置自定义数据库连接对象（放弃内置对象连接池技术）           |
| UseAutoSyncStructure                  | this        | 【开发环境必备】自动同步实体结构到数据库，程序运行中检查实体创建或修改表结构 |
| UseNoneCommandParameter               | this        | 不使用命令参数化执行，针对 Insert/Update，也可临时使用 IInsert/IUpdate.NoneParameter() |
| UseGenerateCommandParameterWithLambda | this        | 生成命令参数化执行，针对 lambda 表达式解析                   |
| UseLazyLoading                        | this        | 开启延时加载功能                                             |
| UseMonitorCommand                     | this        | 监视全局 SQL 执行前后                                        |
| UseNameConvert                        | this        | 自动转换名称 Entity -> Db                                    |
| UseExitAutoDisposePool                | this        | 监听 AppDomain.CurrentDomain.ProcessExit/Console.CancelKeyPress 事件自动释放连接池 (默认true) |
| Build<T>                              | IFreeSql<T> | 创建一个 IFreeSql 对象，注意：单例设计，不要重复创建         |

# [#](http://freesql.net/guide/getting-started.html#connectionstrings)ConnectionStrings

| DataType                           | ConnectionString                                             |
| ---------------------------------- | ------------------------------------------------------------ |
| DataType.MySql                     | Data Source=127.0.0.1;Port=3306;User ID=root;Password=root; Initial Catalog=cccddd;Charset=utf8; SslMode=none;Min pool size=1 |
| DataType.PostgreSQL                | Host=192.168.164.10;Port=5432;Username=postgres;Password=123456; Database=tedb;Pooling=true;Minimum Pool Size=1 |
| DataType.SqlServer                 | Data Source=.;User Id=sa;Password=123456;Initial Catalog=freesqlTest;TrustServerCertificate=true;Pooling=true;Min Pool Size=1 |
| DataType.Oracle                    | user id=user1;password=123456; data source=//127.0.0.1:1521/XE;Pooling=true;Min Pool Size=1 |
| DataType.Sqlite                    | Data Source=\|DataDirectory\|\document.db; Attachs=xxxtb.db; Pooling=true;Min Pool Size=1 |
| DataType.Firebird                  | database=localhost:D:\fbdata\EXAMPLES.fdb;user=sysdba;password=123456 |
| DataType.MsAccess                  | Provider=Microsoft.Jet.OleDb.4.0;Data Source=d:/accdb/2003.mdb |
| DataType.Dameng(达梦)              | server=127.0.0.1;port=5236;user id=2user;password=123456789;database=2user;poolsize=5 |
| DataType.ShenTong(神通)            | HOST=192.168.164.10;PORT=2003;DATABASE=OSRDB;USERNAME=SYSDBA;PASSWORD=szoscar55;MAXPOOLSIZE=2 |
| DataType.KingbaseES(人大金仓)      | Server=127.0.0.1;Port=54321;UID=USER2;PWD=123456789;database=TEST;MAXPOOLSIZE=2 |
| DataType.OdbcMySql                 | Driver={MySQL ODBC 8.0 Unicode Driver}; Server=127.0.0.1;Persist Security Info=False; Trusted_Connection=Yes;UID=root;PWD=root; DATABASE=cccddd_odbc;Charset=utf8; SslMode=none;Min Pool Size=1 |
| DataType.OdbcSqlServer             | Driver={SQL Server};Server=.;Persist Security Info=False; Trusted_Connection=Yes;Integrated Security=True; DATABASE=freesqlTest_odbc; Pooling=true;Min Pool Size=1 |
| DataType.OdbcOracle                | Driver={Oracle in XE};Server=//127.0.0.1:1521/XE; Persist Security Info=False; Trusted_Connection=Yes;UID=odbc1;PWD=123456; Min Pool Size=1 |
| DataType.OdbcPostgreSQL            | Driver={PostgreSQL Unicode(x64)};Server=192.168.164.10; Port=5432;UID=postgres;PWD=123456; Database=tedb_odbc;Pooling=true;Min Pool Size=1 |
| DataType.OdbcDameng (达梦)         | Driver={DM8 ODBC DRIVER};Server=127.0.0.1:5236; Persist Security Info=False; Trusted_Connection=Yes; UID=USER1;PWD=123456789 |
| DataType.OdbcKingbaseES (人大金仓) | Driver={KingbaseES 8.2 ODBC Driver ANSI};Server=127.0.0.1;Port=54321;UID=USER2;PWD=123456789;database=TEST |
| DataType.Odbc                      | Driver={SQL Server};Server=.;Persist Security Info=False; Trusted_Connection=Yes;Integrated Security=True; DATABASE=freesqlTest_odbc; Pooling=true;Min pool size= |

# UnitOfWork

UnitOfWork 可将多个仓储放在一个单元管理执行，最终通用 Commit 执行所有操作，内部采用了数据库事务；

```csharp
var connstr = "Data Source=127.0.0.1;Port=3306;User ID=root;Password=root;" + 
  "Initial Catalog=cccddd;Charset=utf8;SslMode=none;Max pool size=10";

static IFreeSql fsql = new FreeSql.FreeSqlBuilder()
  .UseConnectionString(FreeSql.DataType.MySql, connstr)
  .UseAutoSyncStructure(true) //自动同步实体结构到数据库
  .Build(); //请务必定义成 Singleton 单例模式
```

## [#](http://freesql.net/guide/unit-of-work.html#如何使用)如何使用

```csharp
using (var uow = fsql.CreateUnitOfWork()) {
  var songRepo = fsql.GetRepository<Song>();
  songRepo.UnitOfWork = uow;
  var userRepo = fsql.GetRepository<User>();
  userRepo.UnitOfWork = uow;

  songRepo.Insert(new Song());
  userRepo.Update(...);

  uow.Commit();
}
```

参考：[在 asp.net core 中使用 TransactionalAttribute + UnitOfWorkManager 实现多种事务传播](https://github.com/dotnetcore/FreeSql/issues/289)