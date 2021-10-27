# C#反射给字段赋值

obj,对象，fieldName，字段名，value，字段值

```c
public void SetObjectValue(object obj,string fieldname,object value)

{
obj.GetType().GetField(fieldname).SetValue(obj,value);

}
```

