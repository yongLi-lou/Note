# 关于Convert.ChangeType不能转化Nullable类型的解决方案

讲type转化成非nullable类型

```c#
//判断是否是nullable 
if (type.IsGenericType && type.GetGenericTypeDefinition() == typeof(Nullable<>))
            {
    //转化成对应的非nullable
            type=Nullable.GetUnderlyingType(type);
            }
```

