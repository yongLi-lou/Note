# C# 关键字extern用法

　　修饰符用于声明在外部实现的方法。extern 修饰符的常见用法是在使用 Interop 服务调入非托管代码时与 DllImport 属性一起使用；在这种情况下，该方法还必须声明为 static，如下面的示例所示：

　　[DllImport("avifil32.dll")]
　　private static extern void AVIFileInit();

注意
　　extern 关键字还可以定义外部程序集别名，使得可以从单个程序集中引用同一组件的不同版本。

　　将 abstract（C# 参考）和 extern 修饰符一起使用来修改同一成员是错误的。使用 extern 修饰符意味着方法在 C# 代码的外部实现，而使用 abstract 修饰符意味着在类中未提供方法实现。

注意

　　extern 关键字在使用上比在 C++ 中有更多的限制。若要与 C++ 关键字进行比较，请参见 C++ Language Reference 中的 Using extern to Specify Linkage。