# C#实现rabbitmq 延迟队列功能

  最近在研究rabbitmq,项目中有这样一个场景：在用户要支付订单的时候，如果超过30分钟未支付，会把订单关掉。当然我们可以做一个定时任务，每个一段时间来扫描未支付的订单，如果该订单超过支付时间就关闭，但是在数据量小的时候并没有什么大的问题，但是数据量一大轮训数据库的方式就会变得特别耗资源。当面对千万级、上亿级数据量时，本身写入的IO就比较高，导致长时间查询或者根本就查不出来，更别说分库分表以后了。除此之外，还有优先级队列，基于优先级队列的JDK延迟队列，时间轮等方式。但如果系统的架构中本身就有RabbitMQ的话，那么选择RabbitMQ来实现类似的功能也是一种选择。 我们项目中用到了rabbitmq，可以做一个延迟队列完美的解决这个问题。

   rabbitmq本身不具有延时消息队列的功能，但是可以通过TTL(Time To Live)、DLX(Dead Letter Exchanges)特性实现。其原理给消息设置过期时间，在消息队列上为过期消息指定转发器，这样消息过期后会转发到与指定转发器匹配的队列上，变向实现延时队列。利用rabbitmq的这种特性，应该有了一个大概的思路。、

网上搜了一下 **rabbitmq-delayed-message-exchange** 这个插件也可以实现延迟队列的功能。今天介绍的是如何用C#来实现。

首先了解一下TTL和DLX 

> ## **消息的TTL（Time To Live）**

消息的TTL就是消息的存活时间。RabbitMQ可以对队列和消息分别设置TTL。对队列设置就是队列没有消费者连着的保留时间，也可以对每一个单独的消息做单独的设置。超过了这个时间，我们认为这个消息就死了，称之为死信。如果队列设置了，消息也设置了，那么会取小的。所以一个消息如果被路由到不同的队列中，这个消息死亡的时间有可能不一样（不同的队列设置）。这里单讲单个消息的TTL，因为它才是实现延迟任务的关键。

> ## **Dead Letter Exchanges**

Exchage的概念在这里就不在赘述。一个消息在满足如下条件下，会进死信路由，记住这里是路由而不是队列，一个路由可以对应很多队列。

\1. 一个消息被Consumer拒收了，并且reject方法的参数里requeue是false。也就是说不会被再次放在队列里，被其他消费者使用。

\2. 上面的消息的TTL到了，消息过期了。

\3. 队列的长度限制满了。排在前面的消息会被丢弃或者扔到死信路由上。

Dead Letter Exchange其实就是一种普通的exchange，和创建其他exchange没有两样。只是在某一个设置Dead Letter Exchange的队列中有消息过期了，会自动触发消息的转发，发送到Dead Letter Exchange中去。

 首先我建了两个控制台项目一个是生产者，一个是消费者。

生产者代码如下 

```c#
  var factory = new ConnectionFactory() { HostName = "127.0.0.1", UserName = "test", Password = "test" };
            using (var connection = factory.CreateConnection())
            {
                while (Console.ReadLine() != null)
                {
                    using (var channel = connection.CreateModel())
                    {

                        Dictionary<string, object> dic = new Dictionary<string, object>();
                        dic.Add("x-expires", 30000);
                        dic.Add("x-message-ttl", 12000);//队列上消息过期时间，应小于队列过期时间  
                        dic.Add("x-dead-letter-exchange", "exchange-direct");//过期消息转向路由  
                        dic.Add("x-dead-letter-routing-key", "routing-delay");//过期消息转向路由相匹配routingkey  
                        //创建一个名叫"zzhello"的消息队列
                        channel.QueueDeclare(queue: "zzhello",
                            durable: true,
                            exclusive: false,
                            autoDelete: false,
                            arguments: dic);

                        var message = "Hello World!";
                        var body = Encoding.UTF8.GetBytes(message);

                        //向该消息队列发送消息message
                        channel.BasicPublish(exchange: "",
                            routingKey: "zzhello",
                            basicProperties: null,
                            body: body);
                        Console.WriteLine(" [x] Sent {0}", message);
                    }
                }
            }

            Console.ReadKey();
```

消费者代码如下：

```c#
 var factory = new ConnectionFactory() { HostName = "127.0.01", UserName = "test", Password = "test" };

            using (var connection = factory.CreateConnection())
            {
                using (var channel = connection.CreateModel())
                {
                    channel.ExchangeDeclare(exchange: "exchange-direct", type: "direct");
                    string name = channel.QueueDeclare().QueueName;
                    channel.QueueBind(queue: name, exchange: "exchange-direct", routingKey: "routing-delay");

                    //回调，当consumer收到消息后会执行该函数
                    var consumer = new EventingBasicConsumer(channel);
                    consumer.Received += (model, ea) =>
                    {
                        var body = ea.Body;
                        var message = Encoding.UTF8.GetString(body);
                        Console.WriteLine(ea.RoutingKey);
                        Console.WriteLine(" [x] Received {0}", message);
                    };

                    //Console.WriteLine("name:" + name);
                    //消费队列"hello"中的消息
                    channel.BasicConsume(queue: name,
                                         autoAck: true,
                                         consumer: consumer);

                    Console.WriteLine(" Press [enter] to exit.");
                    Console.ReadLine();
                }
            }

            Console.ReadKey();
```

效果 ：

![img](https://images2015.cnblogs.com/blog/349270/201704/349270-20170414153250423-1244515644.png)

在等待了12秒后消费者等到了消息。

![img](https://images2015.cnblogs.com/blog/349270/201704/349270-20170414153316595-1278430663.png)

 

 这样我们就实现了延迟队列的功能了。

 