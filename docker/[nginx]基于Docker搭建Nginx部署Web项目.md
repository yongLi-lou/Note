# [nginx\]基于Docker搭建Nginx部署Web项目

docker的厉害不用多说，

直接开始吧。

 

步骤一：拉取nginx镜像

------

**docker pull nginx**

直接拉取Docker Hub的官方Nginx镜像（类似git bash的git pull，拉取远程仓库的最新内容更新）

**docker images**

查看本地所有的docker镜像

 

步骤二：基于nginx镜像创建容器

------

**docker run --name mynginx -p 80:80 nginx**

启动一个名为mynginx的容器，同时将容器的80端口映射到宿主机（服务器）的80端口上

**docker ps**

查看所有运行中的容器

 

步骤三：访问

------

在浏览器中输入IP地址即可访问到nginx的默认的欢迎页面

因为当访问宿主机（服务器）的80端口时，docker会自动将访问引入mynginx容器中，

利用容器中的nginx配置的相关的服务。

 

步骤四：开始自定义Nginx配置

------

Nginx的配置项很多，需要满足我们的各种需求：

\- 定义nginx.conf配置文件，放置于宿主机（服务器）的/home/nginx目录下

\- 用于include的vhost目录，从而方便管理，放置于宿主机（服务器）的/home/nginx目录下

\- 定义WEB的根目录www，放置于宿主机（服务器）的/home/nginx目录下

\- 创建两个日志追踪文件nginx_error.log和access.log，放置于宿主机（服务器）的/home/nginx/logs目录下

 

现在，我们通过docker run来实现以上需求

**docker run --name nginx-atlascca3 --privileged=true -p 80:80 -v /home/nginx/nginx.conf:/etc/nginx/nginx.conf -v /home/nginx/vhost:/home/nginx/vhost -v /home/nginx/logs/nginx_error.log:/home/nginx/logs/nginx_error -v /home/nginx/www:/home/nginx/www -d nginx**

 

**docker run [OPTIONS] IMAGE [COMMAND] [ARG...]**

-d: 后台运行容器，并返回容器UUID**（常用）**

-i: 以交互模式运行容器，通常与 -t 同时使用

-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用

-v, –volume=[] 给容器挂载存储卷，挂载到容器的某个目录**（常用）**

–privileged=false 指定容器是否为特权容器，特权容器拥有所有的capabilities

–name=”” 指定容器名字（如mynginx），后续可以通过这个容器名字进行容器管理**（常用）**

 

备注：

**在调试过程中，可以使用docker ps查看所有正在运行的容器（查看容器是否创建）**

**若没有创建成功，说明命令执行不成功，可以把命令中的-d选项去掉，即会显示出相关错误信息~**

**另外，利用docker rm 容器ID可以删除一个容器**