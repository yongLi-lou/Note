# [C#反射动态调用dll中的方法，并返回结果](https://www.cnblogs.com/qq4004229/archive/2013/01/30/2882409.html)

```c#
namespace assembly_name  
{  
    public class assembly_class  
    {  
        public string Show_Str(string str)  
        {  
            if (string.IsNullOrEmpty(str))  
                return "你没有传参数进来";  
            else  
                return "有参数，参数是:" + str;  
        }  
    }  
}
```

```c
//加载程序集(dll文件地址)，使用Assembly类   
Assembly assembly = Assembly.LoadFile(AppDomain.CurrentDomain.BaseDirectory + "Bin/App_Code.dll");  
  
//获取类型，参数（名称空间+类）   
Type type = assembly.GetType("assembly_name.assembly_class");  
  
//创建该对象的实例，object类型，参数（名称空间+类）   
object instance = assembly.CreateInstance("assembly_name.assembly_class");  
  
//设置Show_Str方法中的参数类型，Type[]类型；如有多个参数可以追加多个   
Type[] params_type = new Type[1];  
params_type[0] = Type.GetType("System.String");  
//设置Show_Str方法中的参数值；如有多个参数可以追加多个   
Object[] params_obj = new Object[1];  
params_obj[0] = "jiaopeng";  
  
//执行Show_Str方法   
object value = type.GetMethod("Show_Str", params_type).Invoke(instance, params_obj);
```

