# ef core 数据类型 && 表字段名设置

#### HasColumnType

HasColumnType是指定字段类型

```c#
[Column(TypeName = "decimal(18, 2)")]



public decimal Money { get; set; }



等同于



modelBuilder.Entity<SysUser>().Property(e => e.Name).HasColumnType("CHAR(15)");
```

#### HasColumnName

HasColumnName是指定表字段名
比如说属性名字叫Money,但是你希望表字段名全是小写,所以HasColumnName("money")

```c#
modelBuilder.Entity<SysUser>().Property(e => e.Name).HasColumnName("money");
```

