# Asp.NetCore webApi 控制器增加 Swagger 注释
**Swagger接口文档里 Asp.NetCore webApi 控制器注释不显示，只需要进入Startup.cs**

找到：

```c#

c.IncludeXmlComments(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "WebApilication.xml"));
```
更改为：

```c#

c.IncludeXmlComments(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "WebApilication.xml"), true);
```
　　