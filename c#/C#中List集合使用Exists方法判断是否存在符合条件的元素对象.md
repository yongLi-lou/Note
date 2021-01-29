# [C#中List集合使用Exists方法判断是否存在符合条件的元素对象](https://www.cnblogs.com/xu-yi/p/11029653.html)

```c#
List<TestModel> testList = new List<ConsoleApplication1.TestModel>();
 var resultModel = testList.Exists(t => t.Index == 7);
```