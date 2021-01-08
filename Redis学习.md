# Redis学习



REmote DIctionary Server(Redis) 是一个由Salvatore Sanfilippo写的key-value存储系统。

Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

和普通的Key-Value结构不同，Redis的Key支持灵活的数据结构，除strings，还有hashes、lists、sets和sorted sets等结构。正是这些灵活的数据结构，丰富了Redis的应用场景，能满足更多业务上的灵活存储需求。

Redis的数据都保存在内存中，而且底层实现上是自己写了epoll enent loop部分，而没有采用开源的libevent等通用框架，所以读写效率很高。为了实现Redis的持久化，Redis支持定期刷新（可通过配置实现）或写日志的方式来保存数据到磁盘。

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

 **1、Redis的核心概念**

<font color="blue">**「Redis是什么：」**</font>

- MySql/Oracle：是一个关系型的数据库（关系型的数据库中存在表以及表与表之间关联关系
- Redis：是一个no sql的数据库--->还是数据库（非关系型的数据库）
- 非关系型的数据库：简单来说非关系型的数据库不像关系型的数据库那样存在表以及表之间关联关系，非关系型的数据库中只存在键值对
- 非关系型的数据库中实际上，只有键值对形式的存储，就像Map这种集合一样，只存在键值对
- Redis实际上就是一个基于键值对形式的文件存储系统而已

**2、Redis能干什么？**

<font color="blue">**「主要用途：」**</font>

- 百度上面的单点登录（SSO）
- 商城上面的评论
- 商城上面的积分
- 购物车
- 商城上的缓存（主要是指的是：所有用户公用的数据）
- 最新最热商品的计算
- 消息队列的实现
- ...

**3、Redis的特点**

<font color="blue">**「用户管理命令：」**</font>

- 基于内存的（信息是在内存中的 访问的速度特别快）
- 数据结构简单（Key-Value）
- 支持数据的持久化（能够将内存中的数据同步到硬盘）

**4、Redis中的数据类型以及数据类型的使用场景**

<font color="blue">**「数据类型及使用场景：」**</font>

```rides
 String

      应用场景是什么?普通的数据缓存这种一般就放到这里面  应用场景比较广   因为他就是一个键值对的字符串而已

     List(可以重复的)

      应用场景:这个一般用在:博客的关注、博客的评论、博客的积分

     Hash

      应用场景:SSO单点登录

     Set(无序但是不重复)

      应用场景:这个一般用在和List一样但是不重复的   

    SetSort

      应用的场景:最新最热的商品(有序)
```

**5、Redis中的常用命令（数据类型相关）**

<font color="blue">**「常用命令：」**</font>

```
 set key value:    设置值
  
    get key           获取某一类型的值

    keys *            查看所有的键

    
    del key1,key2..    删除一个或者多个数据

    rename 原来的key   现在key的名字    从新给键命名

    keys *  将当前数据库里面的所有键排列出来


    //在Redis中默认有16个数据库  数据库的下标是0-15   默认存储数据的是会存储到 0号数据库里面

    select index    选中某一个数据库

    keys *         支持模糊查询

    exists key      判断当前数据数据库中是否存在某一个特定的key(注意:不是所有数据库  而是当前操作的这个数据库)

    type key       判断值的类型

    expire key 时间(单位秒钟)   设置key的过期时间

    ttl key     查看key的过期时间

    persist  key    (把某一个key设置成永久有效)   

    set key value    存储数据

    mset key value key value  key value    一次性设置多个键值对  注意:中间是没有任何符号的

    mget a1 a2 a3 a4     一次性获取多个值

    append key1 value    在某一个key对应的值上面追加值

    getset key value     先获取key对应的值  然后再将后面的值 赋值给key

    mgetset key value key value   ....    同时为多个键设置值

    incr key    自增1

    incrby key step    每一次在原来的值上面增加 步长

    decr key    自减1

    decrby key step    每一次在原来的值上面减少 step


  Hash数据类型的方法

    Hset field key value   ---->设置相关的值                      HSet user userToken userInfo

    Hget field key value   ---->获取设置的值

    hlen field             ---->获取的是当前hash里面一共的长度

    hmset field key value ...  --->一次性设置多个键值对到某一个名字中

    hsetnx field key value...  --->不存在的时候再来创建这个值和用户

    hkeys *                ---->查看所有的键

    hincrby field key      ---->自增

    hexists field          ----->查看是否存在


  List数据类型对应的值

    lpush key value  ----->向里面添加值

    lset key index value --->修改某一个位置的值

    lpop key         ----->弹栈（获取这个值）

    lrem key count value ---->删除list集合中的值

    
  Set集合

    sadd key value...   ---->向set集合中添加值

    spop key            ---->获取set集合添加进去的值

    smove key           ---->删除key

    
  SortSet 

     zadd key value score ---->表示的是添加元素到key中  score的意思是排序的位置

     zrange value 范围(开始-结束) --->表示的是获取某一范围内的数据

     zrem value :删除某一个值
```

**6、Redis中的数据持久化问题**

<font color="blue">**「持久化方式：」**</font>

- 持久化方式有两种 rdb aof
- 持久化：简单来说就是内存和数据写入到硬盘的过程，就叫数据的持久化
- 如果是内存的数据在断电的情况下，数据会发生丢失，所以我们的内存数据是需要持久化的

<font color="blue">**「rdb模式：」**</font>

```
  rbd模式(开发一般都不用): 是根据我们的时间片来判断什么时候数据和硬盘进行同步  也就是说假设在一定的条件下才会将数据进行持久化(需要满足一定的条件)、rdb模式在使用的时候会首先将内存数据写入到零时文件 、当这个内存的数据写完成的时候 就会删除原来的rdb文件，重新将零时文件中的内容写入到rdb文件中

    1>:条件要成立才写(条件:)

    2>:先写入零时的文件---->删除rdb文件----->写入rdb文件

       因为要将内存中的所有数据写入到零时文件  相对来说需要更加频繁的去操作IO

    rdb模式适合备份


   #表示的是在900秒之内有一个key发生改变那么就要和硬盘同步
   save 900 1
   #在300秒时间之内 如果有10个key发生改变那么就要和硬盘同步
   save 300 10
   #在60秒的时间内如果有10000个key发生改变那么就要和硬盘同步
   save 60 10000
```

<font color="blue">**「aof模式：」**</font>

```
  aof模式:这种模式是相当于在原来的日志基础上来进行追加、实际上就相当于是 只是同步 改变了的内容  未改变的内容不用同步

      他不会频繁的去操作IO

      要使用aof模式:

      appendonly yes

      #只要有一个key发生改变那么立马和后台同步  这种模式呢不会丢数据但是  效率不高  一般不推荐
      # appendfsync always
      # 这个表示每秒钟和硬盘同步一次
      appendfsync everysec
      # 这个和内存的缓冲区有关 缓冲区满了自动同步  没满的话那么就只有等
      # appendfsync no
```

**7、Redis的主从复制问题（配置从服务器）**

<font color="blue">**「主从复制：」**</font>

```
 假设现在我有一种策略：这种策略就是 能够将访问Redis服务器的请求分成两类(读、写)
 
   然后将读放到一部分的服务器上、写这个操作放到另外的一部分的服务器上、这样就能在3W并发上完成服务器的所有的请求
   
   将服务器上的读和写进行分离 又称为读写分离数据库的主从复制实际上完成的最终的功能就是读写分离

   主从复制:实际上指的是  主服务器来实现写，从服务器来实现读、所有的请求都经过主服务器来完成

   主从复制的配置实现
```

<font color="blue">**「主从复制的步骤：」**</font>

```
 clone服务器之后修改slave的IP地址

    修改配置文件(从服务器的配置文件)  

 vim /usr/local/redis/etc/redis.conf 

    第一步:slaveof 主机地址（eg:119.23.220.148）
 
 bind 0.0.0.0

    第一步下:
     
        #slaveof <masterip> <masterport>
        #改的第二个地方
        slaveof 112.74.49.17 6379

    第二步：masterauth<master-password>(这里可以不用设置)
 
 cd /usr/local/redis/bin/
 
 ./redis-cli

    使用info 查看role角色可以知道主服务或者从服务
```

**8、Redis的哨兵模式**

<font color="blue">**「哨兵模式：」**</font>

- 单点问题：简单来说就是一台服务器挂了，所有服务器都不能用了
- 哨兵模式的出现就是为了解决单点问题的

```
 哨兵:站岗的

   哨兵模式实际上是个程序、这个程序呢实际上就是用来检测主服务器的状态的，一旦主服务器挂了、那么从服务器就会进行投票选举，按照预先设计的选举策略，最终选举出新的主服务器
```

<font color="blue">**「哨兵模式的实现：」**</font>

```
  哨兵模式的实现:(哨兵是一个独立的程序和redis本身的运行是没有关联的)

   任意的一台机器上面去启动哨兵的程序(从服务器上)

   1>:quit  shutdown 退出redis

   2>:copy sentinel.xml到etc这个文件下

   3>:修改 sentinel.xml这个文件  

      dir   -> /usr/local/redis/etc(这个是告诉你路径)

      sentinel monitor mymaster ip 端口 投票选举的次数

      sentinel down-after-milliseconds mymaster 5000  多久进行检测一下主节点活着没有

   启动哨兵  就必须跟上 sentinel这个配置文件

   ...server/  ...sentinel.xml --sentinel &
```

**9、Redis的集群模式**

<font color="blue">**「集群搭建流程：」**</font>

```
  1>:你可以找6台机器来进行安装

  2>:你可以找一台机器开6个端口来进行安装

首先要将redis.conf 复制到我们的etc下
 cp /usr/local/redis-4.0.6/redis.conf /usr/local/redis/etc/

第一步骤:

    mkdir -p /usr/local/redis-cluster   //创建文件夹

第二步骤

     mkdir 500*    在刚刚创建的这个文件夹里面创建6台服务器的 配置文件的文件夹
 
第三步骤

     把原来的Redis.cnf文件copy到我们的500*当中去

第四步骤

     修改配置
 
     daemonize yes

     port 500*

     bind 192.168.108.135 改成 0.0.0.0

     dir "/usr/local/redis-cluster/500*/" 最好打上""

     cluster-enabled yes 这个需要打开注释

     cluster-config-file nodes-500*.conf 这个需要打开注释

     cluster-node-timeout 5000 这个也需要打开注释

     appendonly yes


 第五步骤

   安装ruby的相关工具

   yum install ruby

   yum install rubygems

   gem install redis  (报错版本低了就执行 安装rvm到第9步骤....)

   安装rvm

    gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

    \curl -sSL https://get.rvm.io | bash -s stable

    然后进入cd /usr/local/目录下看是否多个了rvm
 
    然后进入 /usr/local/rvm/archives

   再进行压缩文件的解压  tar-zxvf rvm-1.29.7.tgz

   3.source /usr/local/rvm/archives/rvm-1.29.7/scripts/rvm 添加一个软连接 相当于windows创建一个快捷方式，相当于windows添加一个环境变量,找到这个命令的

    4. 查看rvm库中已知的ruby版本

    rvm list known

    5. 安装一个ruby版本

    rvm install 2.3.3

    6. 使用一个ruby版本

    rvm use 2.3.3

    7. 设置默认版本

    rvm remove 2.0.0

    8. 卸载一个已知版本

    ruby --version

    9. 再安装redis就可以了

    gem install redis   
  
    开启每一个服务器

 去阿里云服务器设置端口，并且服务器重启下

     ./redis-server /usr/local/redis-cluster/700*/redis.conf

     创建这个集群
 进入cd /usr/local/redis-4.0.6/src/这个目录中

  ./redis-trib.rb create --replicas 1 120.78.191.34:7001 120.78.191.34:7002 120.78.191.34:7003 120.78.191.34:7004 120.78.191.34:7005 120.78.191.34:7006

redis-cli --cluster create 106.54.13.167:5001 106.54.13.167:5002 106.54.13.167:5003 106.54.13.167:5004 106.54.13.167:5005 106.54.13.167:5006 --cluster-replicas 1

    登录客户端实现验证
  ./redis-cli -c -h 192.168.108.137 -p 5001

 Jedis进行单单机的访问/连接池下的访问/访问集群

   Redis数据库的访问是要依赖于Jedis
```