

# ef 创建索引

```c#
[Index(nameof(Url))]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .HasIndex(b => b.Url);
}
```



## 複合索引

索引也可以跨越一個以上的資料行：

```c#
[Index(nameof(FirstName), nameof(LastName))]
public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Person>()
        .HasIndex(p => new { p.FirstName, p.LastName });
}
```



## 索引唯一性

根據預設，索引不是唯一的：多個資料列可以有相同的值， (s) 用於索引的資料行集。 您可以建立唯一的索引，如下所示：

```c#
[Index(nameof(Url), IsUnique = true)]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .HasIndex(b => b.Url)
        .IsUnique();
}
```

## 索引名稱

依照慣例，在關係資料庫中建立的索引會命名為 `IX_<type name>_<property name>` 。 如果是複合索引， `<property name>` 則會變成以底線分隔的屬性名稱清單。

您可以設定在資料庫中建立的索引名稱：

```c#
[Index(nameof(Url), Name = "Index_Url")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .HasIndex(b => b.Url)
        .HasDatabaseName("Index_Url");
}
```



## 索引篩選

某些關係資料庫可讓您指定篩選或部分索引。 這可讓您只編制資料行值的子集索引，減少索引的大小，並同時改善效能和磁碟空間使用量。 如需 SQL Server 篩選索引的詳細資訊， [請參閱檔](https://docs.microsoft.com/zh-tw/sql/relational-databases/indexes/create-filtered-indexes)。

您可以使用流暢的 API 來指定索引的篩選準則，提供為 SQL 運算式：

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .HasIndex(b => b.Url)
        .HasFilter("[Url] IS NOT NULL");
}
```

使用 SQL Server 提供者 EF 時 `'IS NOT NULL'` ，會針對屬於唯一索引一部分的可為 null 的資料行加入篩選。 若要覆寫此慣例，您可以提供 `null` 值。

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .HasIndex(b => b.Url)
        .IsUnique()
        .HasFilter(null);
}
```

## 包含的資料行

某些關係資料庫可讓您設定一組包含在索引中的資料行，但不是其「索引鍵」的一部分。 當查詢中的所有資料行都包含在索引中做為索引鍵或非索引鍵資料行時，這可大幅提升查詢效能，因為資料表本身不需要存取。 如需 SQL Server 內含資料行的詳細資訊， [請參閱檔](https://docs.microsoft.com/zh-tw/sql/relational-databases/indexes/create-indexes-with-included-columns)。

在下列範例中，資料 `Url` 行是索引鍵的一部分，因此該資料行上的任何查詢篩選都可以使用索引。 此外，只存取和資料行的 `Title` 查詢 `PublishedOn` 不需要存取資料表，而且會更有效率地執行：

```c#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Post>()
        .HasIndex(p => p.Url)
        .IncludeProperties(
            p => new { p.Title, p.PublishedOn });
}
```

