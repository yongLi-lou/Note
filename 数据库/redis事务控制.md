1.说明：现在假设有一个session要执行若干次的修改处理，就可以考虑将若干操作放到一个事务中，这样处理可以让这些操作仪器一起执行或一起取消。Redis中的事务控制没有传统SQL中的事务控制那样智能，redis中事务控制是将所有可以执行的指令都进行执行，不能执行的指令报错。

2.指令：

• 开启事务：multi 

• 提交事务：exec

• 取消当前队列的所有操作：discard

举例说明：

```
eg1:
set age 18
multi
set age 99
get age   这时执行结果为18，因为事务开启了，set age 99没有提交。
exec
 
eg2:
set age 10
set info lee
multi
incr age
incr info(info是字符串不能执行自增操作)
exec
执行结果：age的会执行，而incr info的会报错，这就体现出了不是一起成功一起失败<br><br>eg3：<br>multi<br>set name zhangsan<br>getset age 18<br>这时会报错，因为getset不是指令，这一组操作会全部失败

```

• 监控一个或多个key：watch 类似乐观锁，配合multi使用，一直监控着有没有人修改，如果中途有其他人修改则会提交失败。

• 取消对所有key的监控：unwatch，如果watch时提交失败，我们需要执行unwatch先取消监控，再执行watch再次监控，multi，最后再提交

3.redis事务的三个阶段：开启，入队，执行

4.redis事务的三个特性：单独的隔离操作，没有隔离级别的概念，不保证原子性

3.lettuce事务控制：

 • 同步的事务处理机制

```
package com.yootk.lettuce.transaction;
import com.yootk.lettuce.util.RedisConnectionUtil;
import io.lettuce.core.api.StatefulRedisConnection;
import io.lettuce.core.api.sync.RedisCommands;
import java.util.concurrent.TimeUnit;
 
public class SyncTransactionDemo {
    public static void main(String[] args) throws Exception {
        StatefulRedisConnection connection = RedisConnectionUtil.getConnection();   // 获取连接
        RedisCommands commands = connection.sync();//  创建同步操作命令
        System.out.println("【开启事务】开启多行执行：" + commands.multi());
        System.out.println("【设置数据】" + commands.set("lee-msg","Hello World"));
        System.out.println("【设置数据】" + commands.set("black-msg","Black Face"));
        TimeUnit.SECONDS.sleep(10); // 追加一个延迟处理
        System.out.println("【事务提交】" + commands.exec()); // 执行多个语句
        RedisConnectionUtil.close();
    }
 
}
```

 • 异步的事务处理机制

```
package com.yootk.lettuce.transaction;
import com.yootk.lettuce.util.RedisConnectionUtil;
import io.lettuce.core.RedisFuture;
import io.lettuce.core.TransactionResult;
import io.lettuce.core.api.StatefulRedisConnection;
import io.lettuce.core.api.async.RedisAsyncCommands;
import io.lettuce.core.api.sync.RedisCommands;
import java.util.concurrent.TimeUnit;
 
public class AsyncTransactionDemo {
    public static void main(String[] args) throws Exception {
        StatefulRedisConnection connection = RedisConnectionUtil.getConnection();   // 获取连接
        RedisAsyncCommands commands = connection.async();   // 执行语句
        RedisFuture multi = commands.multi();// 开启事务
        RedisFuture setCmdA = commands.set("lee-msg", "Hello World");
        RedisFuture setCmdB = commands.set("black-msg","Black Face") ;
        RedisFuture<TransactionResult> exec = commands.exec();  // 自动的执行事务提交
        System.out.println("【开启事务】开启多行执行：" + multi.get());
        System.out.println("【设置数据】" + setCmdA.get());
        System.out.println("【设置数据】" + setCmdB.get());
        TimeUnit.SECONDS.sleep(5); // 追加一个延迟处理
        System.out.println("【事务提交】" + exec.get()); // 返回执行结果
        RedisConnectionUtil.close();
    }
 
}<br>• 响应式的事务处理机制
```



```
package com.yootk.lettuce.transaction;
import com.yootk.lettuce.util.RedisConnectionUtil;
import io.lettuce.core.api.StatefulRedisConnection;
import io.lettuce.core.api.reactive.RedisReactiveCommands;
import java.util.concurrent.TimeUnit;

public class ReactiveTransactionDemo {
    public static void main(String[] args) throws Exception {
        StatefulRedisConnection connection = RedisConnectionUtil.getConnection();   // 获取连接
        RedisReactiveCommands commands = connection.reactive();// 创建响应式命令
        commands.multi().subscribe(temp ->{
            System.out.println("【设置数据】" + commands.set("lee-msg", "Hello World").subscribe());
            System.out.println("【设置数据】" + commands.set("black-msg","Black Face").subscribe());
            try {
                TimeUnit.SECONDS.sleep(10); // 休眠10秒
            } catch (InterruptedException e) {
            }
            System.out.println("【事务提交】" + commands.exec().subscribe());
        }) ;
        TimeUnit.SECONDS.sleep(20); // 追加一个等待机制
        RedisConnectionUtil.close();
    }
}
```



4.SpringDataRedis事务控制:在SpringDataRedis里面，所有的数据库操作都是通过RedisTemplate展开的，相应的事务处理也可以通过此类直接完成处理。

```
import` `java.util.concurrent.TimeUnit;``@ContextConfiguration``(locations={``"classpath:spring/*.xml"``})``@RunWith``(SpringJUnit4ClassRunner.``class``)``public` `class` `TestRedisTemplateTransaction{`` ``@Autowired`` ``private` `RedisTemplate<String,String>  stringRedisTemplate;`` ``@Test`` ``public` `void` `testTransaction()``throws` `Exception{``   ``this``.stringRedisTemplate.setEnableTransactionSupport(``true``);``//开启事务支持``   ``this``.stringRedisTemplate.multi();``//开启事务操作``   ``this``.stringRedisTemplate.opsForValue().set(``"lee-msg"``,``"hello world"``);``   ``this``.stringRedisTemplate.opsForValue().set(``"black-msg"``,``"black face"``); ``   ``TimeUnit.SECONDS.sleep(``5``);``   ``System.out.println(``"【执行事务】"``+``this``.stringRedisTemplate.exec());``//提交`` ``}``}
```

　　