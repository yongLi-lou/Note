# net Core 红隼（Kestrel）使用

### netcore3.1/net5

```c#
public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                    webBuilder.UseKestrel();//使用Kestrel服务
                    webBuilder.UseUrls("http://localhost:8080");//配置端口号
                    
                    
                });
    }
```



### net6/net7

```c#
```

