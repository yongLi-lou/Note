# 使用nginx后net core无法获取ip问题

使用了nginx后net core获取ip地址居然全部是本地的地址，不是外网的地址

这是因为nginx转发了一次后，我们直接使用常规获取ip地址的方式就是本地的地址了



瞧瞧nginx的配置,然后找获取外网ip的方法



 ![img](https://img-blog.csdnimg.cn/20200513110912222.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0ODk3NzQ1,size_16,color_FFFFFF,t_70)

这里我们可以看到，我们配了一个real-ip,nginx会转发给你，通过请求的header获取就行了 

context.HttpContext.Request.Headers["X-Real-IP"].FirstOrDefault();

