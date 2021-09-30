# NetCore偶尔有用篇：NetCore项目发布为Nuget包

**一、简介**

------

1、nuget大家已经不陌生。

2、netcore默认引用便是nuget，并处理了嵌套关系。

3、netcore已经支持直接编译生成nuget包。

4、本文介绍如何把自己建立的项目发布为nuget程序包。

注意：netCore下的nuget包仅能包含dll，不支持任何静态文件。

**二、准备工作**

------

1、去nuget官方注册一个账号，也可以直接用微软账号登录。[去注册](https://www.nuget.org/packages)

2、没有了。

**三、创建项目生成nuget包**

------

1、创建一个类库项目。

2、写代码，这个自己随便写了。

3、配置nuget包：在项目配置属性->打包设置。

4、生成nuget包：到bin下面可以看到nuget包。

 

![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508145819134-1188305533.png)

![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508145834283-686522817.png)

 ![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508145839195-1874287705.png)

 ![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508145844810-736644816.png)

**四、上传发布**

------

1、上传编辑好的包。[上传地址](https://www.nuget.org/packages/manage/upload)

2、等待后台处理，处理后会显示在列表中。

3、在vs中查看，引用。

![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508150420498-1680838426.png)

![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508150427539-975886906.png)

![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508150434813-23156295.png)

![img](https://images2018.cnblogs.com/blog/1006345/201805/1006345-20180508150440550-569154254.png)