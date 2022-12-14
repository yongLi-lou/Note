# python中的类型提示(定义函数时加入箭头->)

比如：

```python
def add(a:int, b:int) -> int:



    return a+b
```

这个表示并没有多么的神奇，意思是：**告诉你期待的输入类型和输出类型**。上面代码期待的类型为int。

其实就是变量类型的动态定义和静态定义的区别。同样一个函数可以不加->表示动态定义和加->表示静态定义。

对于上面左边函数，对n的数据类型不一定为int，也可以为float等等。。而右边限定了只能int。

这就是动静态的区别。

我试着寻找这两者的区别和各自优势。有以下发现：

1. 将动态类型函数改为静态类型函数并不能使计算加快；

2. 就算你静态限定了int，输入为float的时候也不会报错，输出也不会变成期待的int类型。所以在使用上，动静态类型并没有区别。

那么这个type hint看起来是比较鸡肋。

它的用处有以下：

1. 增加代码可读性；

2. 比较容易用其他语言改写。
