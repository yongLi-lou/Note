## [vue下使用nginx刷新页面404](https://www.cnblogs.com/JiAyInNnNn/p/11204995.html)

nginx 是一个代理的服务器。
出现的问题：写好的页面通过nginx作为代理的服务器给别的同事看的时候发现了新写的页面打开就404，并且从其他页面跳转可以看到但是刷新页面就404。
解决方法：
在文件中的nginx.conf文件中修改，代码如下

```
server {
        listen       YYYY;    //自己设置的端口号
        server_name  192.168.XXX.XXX;   //在黑窗口下ipconifg后出现的IPv4地址复制
         
        location /{
            root E:/website_wap/dist/;   //项目打包后的路径
            index index.html index.htm;
            try_files $uri $uri/ /index.html;    //解决刷新页面变成404问题的代码
        }  
    }
```

　　

访问的地址就是192.168.XXX.XXX:YYYY

一定要记得先npm run build 哦~（我就是在凑字数！！）

解决代码：

```
 try_files $uri $uri/ /index.html;   
```