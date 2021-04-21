# python的class(类)中的object是什么意思?

那写object和不写object有什么区别？

好的，再用代码来理解它们的区别.

```python
# -.- coding:utf-8 -.-
# __author__ = 'zhengtong'
 
 
class Person:
    """
    不带object
    """
    name = "zhengtong"
 
 
class Animal(object):
    """
    带有object
    """
    name = "chonghong"
 
if __name__ == "__main__":
    x = Person()
    print "Person", dir(x)
 
    y = Animal()
    print "Animal", dir(y)
```

运行结果

```python
Person ['__doc__', '__module__', 'name']
Animal ['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', 
'__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', 
'__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'name']
```

Person类很明显能够看出区别，不继承object对象，只拥有了__doc__ , __module__ 和 自己定义的name变量, 也就是说这个类的命名空间只有三个对象可以操作.

Animal类继承了object对象，拥有了好多可操作对象，这些都是类中的高级特性。

 

对于不太了解python类的同学来说，这些高级特性基本上没用处，但是对于那些要着手写框架或者写大型项目的高手来说，这些特性就比较有用了，比如说tornado里面的异常捕获时就有用到__class__来定位类的名称，还有高度灵活传参数的时候用到__dict__来完成.

 

最后需要说清楚的一点， 本文是基于python 2.7.10版本，实际上在python 3 中已经默认就帮你加载了object了（即便你没有写上object）。



