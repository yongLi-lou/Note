# C# 两个DateTime比较

datetime比较用long类型。使用Ticks ()方法转换成long

```c#
Dt>=DateTime.Parse(Strtime1begin).Ticks && Dt<=DateTime.Parse(Strtime1end).Ticks
```

