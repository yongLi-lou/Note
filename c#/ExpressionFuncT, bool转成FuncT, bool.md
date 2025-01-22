# Expression<Func<T, bool>>转成Func<T, bool>

```c#
 // 定义一个表达式树
        Expression<Func<AppUserCouponDto, bool>> expression = x => x.Id > 10 && x.IsActive;

        // 使用 Compile 将 Expression<Func<T, bool>> 转换成 Func<T, bool>
        Func<AppUserCouponDto, bool> compiledFunc = expression.Compile();

        // 测试
        var dto = new AppUserCouponDto { Id = 15, IsActive = true };
        bool result = compiledFunc(dto);

        Console.WriteLine($"Result: {result}"); // 输出: Result: True
```

