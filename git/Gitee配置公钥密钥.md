# Gitee配置公钥密钥

使用Git相关的功能时，一般都需要代码在网络间传输，https每次都需要输入账号密码，所以配置下ssh会好一点。
我用的是Mac os，ssh证书是默认存放在 ~/.ssh 目录下的，Linux系统也是如此。默认的ssh-key文件是：id_rsa 和 id_rsa.pub。
普通生成rsa方式（用于多个服务器之间的登录）
————————————————
版权声明：本文为CSDN博主「liyanpig」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

```python
ssh-keygen -t rsa

```

针对某个网站的rsa生成方式（我将使用gitee，这里引用了gitee的图片）

```python
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501164045730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeWFucGln,size_16,color_FFFFFF,t_70)

使用方式
将本机~/.ssh/id_rsa.pub内容拷贝到框框内

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501164101484.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050116411333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeWFucGln,size_16,color_FFFFFF,t_70)

本机连接gitee就可以使用ssh方式了

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020050116413587.png)

ssh免密的原理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501164142519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpeWFucGln,size_16,color_FFFFFF,t_70)