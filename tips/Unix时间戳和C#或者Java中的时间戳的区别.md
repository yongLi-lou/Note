# Unix时间戳和C#或者Java中的时间戳的区别

1.时间戳的区别

　　Unix时间戳是从1970年1月1日（UTC/GMT的午夜）开始所经过的秒数，不考虑闰秒。

　　但是`Java`中很多获取时间戳的`API`并不是获取到`Unix`时间戳，而是获取到***从1970年1月1日（UTC/GMT的午夜）开始所经过的毫秒数***。以毫秒计算的时间戳下面统一称为**时间戳**。



