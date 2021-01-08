# EF Core使用整理

<font color="">**一、安装Nuget包**</font>

**Install-package Microsoft.EntityFrameworkCore**
 **Install-package Microsoft.EntityFrameworkCore.SqlServer**

**Micorsoft.EntityFrameworkCore：EF框架的核心包**

**Micorsoft.EntityFrameworkCore.SqlServer:针对SqlServer数据库的扩展，使用SqlServer数据库必须。类似的还有MySql，SqlLite等**

> 一、CodeFirst 
>
> > > 1. 手动创建上下文注入sql语句
> > > 2. 手动写实体类
> > > 3. 打开nuget包控制台 `add-migration 【name】`-》`update database`
> > > 4. 或者打开cmd `dotnet ef migrations add [name]`-》`dotnet ef database update`(dot net 以上版本)更新dot net 指令：`dotnet tool update --global dotnet-ef --version 3.0.0-preview7.19362.6`
>
> 二、DatabaseFirst
>
> > > 1. 打开包控制台
> > >
> > > 2.  输入`Scaffold-DbContext "server=localhost;port=3306;database=login;uid=root;pwd=123456" Pomelo.EntityFrameworkCore.MySql -OutputDir Models//database first `
> > >
> > > 3. 如果是sqlserver则把包改成**Microsoft.EntityFrameworkCore.SqlServer**并且引用Micorsoft.EntityFrameworkCore.Tools
> > >
> > >    &Micorosft.EntityFrameworkCore.Design
> > >
> > > **`Scaffold-DbContext`命令详解**：
> > >
> > > scaffold-dbcontext（数据库上下文脚手架）指令来生成models和context。
> > >
> > > Scaffold-DbContext [-Connection] <String> [-Provider] <String> [-OutputDir <String>] [-Context <String>] 
> > > [-Schemas <String>] [-Tables <String>] [-DataAnnotations] [ -Force] [-Project <String>] 
> > > [-StartupProject <String>] [-Environment <String>] [<CommonParameters>]
> > >
> > > PARAMETERS 
> > > -Connection <String> 
> > > 指定数据库的连接字符串。
> > >
> > > -Provider <String> 
> > > 指定要使用的提供程序。例如，Microsoft.EntityFrameworkCore.SqlServer。
> > >
> > > -OutputDir <String> 
> > > 指定用于输出类的目录。如果省略，则使用顶级项目目录。
> > >
> > > -Context <String> 
> > > 指定生成的DbContext类的名称。
> > >
> > > -Schemas <String> 
> > > 指定要为其生成类的模式。
> > >
> > > -Tables <String> 
> > > 指定要为其生成类的表。
> > >
> > > -DataAnnotations [<SwitchParameter>] 
> > > 使用DataAnnotation属性在可能的情况下配置模型。如果省略，输出代码将仅使用流畅的API。
> > >
> > > -Force [<SwitchParameter>] 
> > > 强制脚手架覆盖现有文件。否则，只有在没有输出文件被覆盖的情况下，代码才会继续。
> > >
> > > -Project <String> 
> > > 指定要使用的项目。如果省略，则使用默认项目。
> > >
> > > -StartupProject <String> 
> > > 指定要使用的启动项目。如果省略，则使用解决方案的启动项目。
> > >
> > > -Environment <String> 
> > > 指定要使用的环境。如果省略，则使用“开发”。

## 目前遇到的问题：

1. 执行ef-Core创建不同上下文迁移报错More than one DbContext was found

原因：

由于使用的多个DbContext，而在使用命令生成数据库文件时又没有指定是执行哪个DbContext

（使用命令是指定是在哪个数据库执行）

如在原命令后加上代码-c DbContext的文件名，如下

执行命令生成创建的文件

dotnet ef migrations add Initial -c DemoDbContext

执行命令生成数据库

dotnet ef database update -c DemoDbContext