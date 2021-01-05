# Redis学习

REmote DIctionary Server(Redis) 是一个由Salvatore Sanfilippo写的key-value存储系统。

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。

## 数据基础类型

* string 字符串
* Hash 散列
* List 列表
* Set 集合
* Sorted Set有序集合

## Windows 下载地址

https://github.com/tporadowski/redis/releases。

打开cmd窗口 使用cd切换到C：redis目录运行：

```Redis
redis-server.exe redis.windows.conf
```

这时候另启一个 cmd 窗口，原来的不要关闭，不然就无法访问服务端了。
切换到Redis

```redis
redis-cli.exe -h 127.0.0.1 -p 6379
```

设置键值对：

```redis
set myKey abc
```

取出键值对：

```redis
get myKey
```

## Redis配置

```Redis
redis 127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME （* 获取所有）
```