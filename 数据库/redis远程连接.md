# redis远程连接

```
$ redis-cli -h host -p port -a password

-h 服务器地址 -p 端口号 -a 密码
```

远程连接报错如下：

192.168.153.131:6379> set 1 1
(error) DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.

解决方案：

下载redis目录找到redis.conf，打开配置找到bind 120.0.0.1，增加bind 192.168.153.131，此处192.168.153.131是本机的ip地址，linux可通过ifconfig查看ip地址
