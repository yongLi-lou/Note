# SQL server修改数字长度时算术溢出

在 SQL Server 中，`numeric(18,6)` 和 `numeric(18,10)` 都是 `numeric` 类型的数据，其中 `18` 是总精度，后面的数字是小数位数。当你将 `numeric(18,6)` 改为 `numeric(18,10)` 时，你实际上是在增加小数位数，但这会减少整数位数。

原来的 `numeric(18,6)` 可以存储最大为 `999999999999.999999` 的数值，其中整数部分可以有 `12` 位，小数部分可以有 `6` 位。但当你改为 `numeric(18,10)` 后，虽然小数位数增加到 `10` 位，但整数部分就只剩下 `8` 位了，所以最大只能存储 `99999999.9999999999` 的数值。

#### 长度和精度需要一起加