# MySQL性能优化之char、varchar、text的区别

   在存储字符串时， 可以使用char、varchar或者text类型， 那么具体使用场景呢？

1、 char长度固定， 即每条数据占用等长字节空间；适合用在身份证号码、手机号码等定。

2、 varchar可变长度，可以设置最大长度；适合用在长度可变的属性。

3、 text不设置长度， 当不知道属性的最大长度时，适合用text。



按照查询速度： char最快， varchar次之，text最慢。



char：char(n)中的n表示字符数，最大长度是255个字符； 如果是utf8编码方式， 那么char类型占255 * 3个字节。（utf8下一个字符占用1至3个字节）



varchar：varchar(n)中的n表示字符数，最大空间是65535个字节， 存放字符数量跟字符集有关系；

     MySQL5.0.3以前版本varchar(n)中的n表示字节数；
    
     MySQL5.0.3以后版本varchar(n)中的n表示字符数；



  PS：varchar实际范围是65532或65533， 因为内容头部会占用1或2个字节保存该字符串的长度；如果字段default null（即默认值为空），整条记录还需要1个字节保存默认值null。

       如果是utf8编码， 那么varchar最多存65532/3 = 21844个字符。



text：



跟varchar基本相同， 理论上最多保存65535个字符， 实际上text占用内存空间最大也是65535个字节； 考虑到字符编码方式， 一个字符占用多个字节， text并不能存放那么多字符； 跟varchar的区别是text需要2个字节空间记录字段的总字节数。

PS： 由于varchar查询速度更快， 能用varchar的时候就不用text。

顺便提一句： 当表有成百上千万条数据时， 就要使用MySQL的分区(partition)功能， 原理有点像分治算法，就是将数据切割成多个部分。

总结起来，有几点：



1. 经常变化的字段用varchar
2. 知道固定长度的用char
3. 尽量用varchar
4. 超过255字符的只能用varchar或者text
5. 能用varchar的地方不用text