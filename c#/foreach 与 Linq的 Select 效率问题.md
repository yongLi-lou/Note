# foreach 与 Linq的 Select 效率问题

Resharper 是一个非常强大的C#编程辅助工具，有着非常强的提示功能，代码纠正，代码简化等等

在编码过程中注意到这么一件事，可能是大家经常会遇到的：

遍历某个集合，然后经过处理生成另外一个集合，例如遍历一个产品列表，生成一个SelectItem 的List，我一直这么写：

```c#
var list = new List<SelectListItem>();

            foreach (var product in products)
            {
                list.Add(new SelectListItem(){ Text = product.Name, Value = product.Id});
            }
```

这时，Resharper会提示我将它改成这样：

```c#
var list = products.Select(product => new SelectListItem() {Text = product.Name, Value = product.Id}).ToList();
```

是简洁了不少，我也开始喜欢这种风格

但在随后的一段代码中，处理上百万的数据，运行比较慢，就开始着手优化代码，**发现直接Foreach比Select要快那么一点点，想想也能明白，select内部的实现逻辑，会转换为函数调用，效率肯定会比直接做循环要低**。

有时候，简介的代码也许不能带来高的效率，得视情况而定，小数据量，可以Select保持代码简介，对与大量数据，还是直接做简单的循环，简单的数据结构，效率会更好。



**for和foreach在.net4.5以后没太多差别**

