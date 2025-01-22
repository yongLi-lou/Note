```
IEnumerable<IEnumerable<int>>` 转换为一个 `List<int>`，可以使用 `SelectMany
```

```c#
var payInfoIds = m.costs
    .SelectMany(e => e.pay_info_ids.Select(e => e.pay_info_id))
    .ToList();

```

