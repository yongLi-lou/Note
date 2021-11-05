# C#反射给字段赋值

obj,对象，fieldName，字段名，value，字段值

```c
public void SetObjectValue(object obj,string fieldname,object value)

{
obj.GetType().GetField(fieldname).SetValue(obj,value);

}


```

```c#
//首字母大写
var fieldname = System.Threading.Thread.CurrentThread.CurrentCulture.TextInfo.ToTitleCase(item.FieldName);
                    Type t = asset.GetType();
                   //给属性赋值
                   var prop= t.GetProperty(fieldname);
                    prop.SetValue(asset,item.End);
```

