# StackExchange.Redis事件和事件参数

### **ConnectionMultiplexer 类型公开了多个事件：**

>- **ConfigurationChanged** 当     ConnectionMultiplexer 里面的连接配置被更改后触发
>- **ConfigurationChangedBroadcast**     通过发布/订阅功能接受到一个重新配置的消息的时候；这通常是由于使用 IServer.MakeMaster     更改了一个节点的复制配置，也可以选择广播某个请求给所有的客户。
>- **ConnectionFailed** 当连接失败的时候；注意：对于该连接你不会收到     ConnectionFailed 的通知，直到连接重新建立。
>- **ConnectionRestored** 当重新建立连接到之前失败的那个节点的时候
>- **ErrorMessage**     当用户发起的请求的时候，Redis服务器给出一个错误消息的响应；这是除了常规异常/故障之外，立即报告给调用方的。
>- **HashSlotMoved** 当 “Redis集群” 表示 hash-slot     已经被迁移到节点之间的时候，注意：请求通常会被自动重新路由，所以用户不会在这里要求做任何指定的事情。
>- **InternalError**     当Redis类库内部执行发生了某种不可预期的失败的时候；这主要是为了用于调试,大多数用户应该不需要这个事件。

### 参数：

>**ConfigurationChange** **参数：** **EndPointEventArgs**

> **ConfigurationChangedBroadcast** **参数：** **EndPointEventArgs**

> **ConnectionFailed** **参数：** **ConnectionFailedEventArgs**

> **ConnectionRestored** **参数：** **ConnectionFailedEventArgs**

> **ErrorMessage** **参数** **:RedisErrorEventArgs**

> **HashSlotMoved** **参数：** **HashSlotMovedEventArgs**

> **InternalError**  **参数** **:InternalErrorEventArgs**



## 举例：

DESKTOP-85CKU9T连接失败[2020/12/23 04:27:23:59]Endpoint failed: Unspecified/r-bp1o34m0xnofw53evopd.redis.rds.aliyuncs.com:6379, SocketFailure, SocketFailure (ReadSocketError/TimedOut, last-recv: 35) on r-bp1o34m0xnofw53evopd.redis.rds.aliyuncs.com:6379/Interactive, Idle/Faulted, last: INFO, origin: ReadFromPipe, outstanding: 20, last-read: 20s ago, last-write: 0s ago, keep-alive: 60s, state: ConnectedEstablished, mgr: 9 of 10 available, in: 0, in-pipe: 0, out-pipe: 0, last-heartbeat: 0s ago, last-mbeat: 0s ago, global: 0s ago, v: 2.2.4.27433

-------------------

--------



 last-recv：上次接收，

ReadFromPipe:从管道读取，

outstanding：停留时间，

 last-heartbeat：最后发送heartbeat信息时间，

last-mbeat：最后发送mbeat信息时间

mgr:Redis内部专用线程池状态

----

----

***<font color=#FF0000>事件参数不能直接ToString()</font>***





