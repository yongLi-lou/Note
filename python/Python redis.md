# Python redis

```python
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True,password='123456')   # host是redis主机，需要redis服务端和客户端都启动 redis默认端口是6379
r.set('Foo:foo','bar')  # key是"foo" value是"bar" 将键值对存入redis缓存
print(r['Foo:foo'])
print(r.get('Foo:foo'))  # 取出键foo对应的值
print(type(r.get('Foo:foo')))
```

```python
import redis

class RedisHelper(object):
    def __init__(self):
        self.__conn = redis.Redis(host='localhost',port=6379,password='123456')#连接Redis
        self.channel = 'monitor' #定义名称

    def publish(self,msg):#定义发布方法
        self.__conn.publish(self.channel,msg)
        return True

    def subscribe(self):#定义订阅方法
        pub = self.__conn.pubsub()
        pub.subscribe(self.channel)
        pub.parse_response()
        return pub
```

