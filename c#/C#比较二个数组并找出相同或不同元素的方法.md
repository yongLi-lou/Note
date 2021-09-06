# [C#比较二个数组并找出相同或不同元素的方法](https://www.cnblogs.com/moy-1313133/p/6648132.html)

```c#
string[] arr1 = new[] { "1", "2", "3", "4", "5" };
string[] arr2 = new[] { "1", "3", "5" };

//找出相同元素(即交集)
var sameArr = arr1.Intersect(arr2).ToArray();

//找出不同的元素(即交集的补集)
var diffArr = arr1.Where(c => !arr2.Contains(c)).ToArray();
```

