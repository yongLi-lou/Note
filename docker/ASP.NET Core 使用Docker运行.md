# ASP.NET Core 使用Docker运行

# 1.新建ASP.NET Core项目

新建一个名为“DockerSample”的ASP.NET Core项目

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224144759115-1063318367.png)

运行程序，页面如下：

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224145138581-1213775875.png)

# 2.编写DockerFile

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224145321669-763206839.png)

目标系统选择Linux

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224145400977-1338838685.png)

此时目录中会自动添加dockerfile文件，文件系统结构如下：

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224145549957-762381771.png)

dockerfile文件内容如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
 2 WORKDIR /app
 3 EXPOSE 80
 4 EXPOSE 443
 5 
 6 FROM microsoft/dotnet:2.1-sdk AS build
 7 WORKDIR /src
 8 COPY ["DockerSample/DockerSample.csproj", "DockerSample/"]
 9 RUN dotnet restore "DockerSample/DockerSample.csproj"
10 COPY . .
11 WORKDIR "/src/DockerSample"
12 RUN dotnet build "DockerSample.csproj" -c Release -o /app
13 
14 FROM build AS publish
15 RUN dotnet publish "DockerSample.csproj" -c Release -o /app
16 
17 FROM base AS final
18 WORKDIR /app
19 COPY --from=publish /app .
20 ENTRYPOINT ["dotnet", "DockerSample.dll"]
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 对其修改如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
1  FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
2  WORKDIR /app
3  
4  COPY . .
5 
6  EXPOSE 80
7  EXPOSE 443
8 
9  ENTRYPOINT ["dotnet", "DockerSample.dll"]
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

修改DockerFile的属性

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224150111573-96918469.png)

 

# 3.发布程序

将程序进行发布

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224151517965-1555717777.png)

发布后的程序目录如下：

 ![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224151454493-2113787705.png)

在发布的目录下打开PowerShell

运行指令编译镜像

```
1 docker build -t dockersample .
```

查看可用的镜像

```
1 docker image ls
```

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224152346606-644887574.png)

 

 运行镜像

 

```
docker run --name=dockersamplel -p 20005:80 -d dockersample
```

 

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224154007619-1626903843.png)

如果运行后出现一串ID，则表示运行正常，80位docker容器的端口，映射到本机的端口号位20080

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224154121752-341187383.png)

 

# 常见错误

1.driver failed programming external connectivity on endpoint dockersample....

![img](https://img2018.cnblogs.com/blog/687946/201812/687946-20181224154438550-1069071191.png)

该错误只需要重启Docker即可

2.测试端口是否占用

```
1  netstat -ano|find ":1433"
```

3.测试Dokcer是否正确安装

```
1 docker run -it hello-world
```