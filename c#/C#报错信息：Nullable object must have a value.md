# C#报错信息：***Nullable object must have a value\***

翻译过来就是：可为空的对象必须具有一个值。

看起来有点怪怪的，既然是可空对象为什么必须要有值吗？下面我们就探讨一下吧....

举个列子，开发过程中可能碰到的一个场景：

有一个类Goods,这个类有很多的属性，其中有一个Price属性为可空double类型，我们需要另外一个类GoodsDto 来接收Goods对象并输出，其中也有一个Price属性

为double类型。我们可以看到报错了 double？转double,因为double？可能是空的，但是double转double?不会报错，因为double永远都有值，而且double?可接受空值

![img](https://img2018.cnblogs.com/blog/1187461/201908/1187461-20190817165508202-890694518.png)

 

那么怎么处理呢？

```c#
GoodsDto dto = new GoodsDto()
   {
       GoodsName = goods.GoodsName,
       GoodsNo = goods.GoodsNo,
       Price = goods.Price.Value
   };　
```

再次运行下，发现报错Nullable object must have a value，什么情况呢？

其实稍微分析一下就不难理解了，回到根源还是double？转double的问题 goods.Price没值的话，goods.Price.Value就是空值， dto.Price是不可能接受的。

虽然你是可空的，但是当我需要的时候你必须要有值。

所以加个判断就好了（为空时给一个默认值）

```c#
           GoodsDto dto = new GoodsDto()
            {
                GoodsName = goods.GoodsName,
                GoodsNo = goods.GoodsNo,
                Price = goods.Price == null ? 0 : goods.Price.Value
            };
```