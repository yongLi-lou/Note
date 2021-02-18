# URL和URI的区别

**URI包括URL和URN两个类别，URL是URI的子集，所以URL一定是URI，而URI不一定是URL**

URI = Universal Resource Identifier 统一资源标志符，用来标识抽象或物理资源的一个紧凑字符串。
URL = Universal Resource Locator 统一资源定位符，一种定位资源的主要访问机制的字符串，一个标准的URL必须包括：protocol、host、port、path、parameter、anchor。
URN = Universal Resource Name 统一资源名称，通过特定命名空间中的唯一名称或ID来标识资源。

![url_uri_diff](https://images2015.cnblogs.com/blog/591228/201601/591228-20160116223301225-1866838315.png)

**举个栗子：**

> 个人的身份证号就是URN，个人的家庭地址就是URL，URN可以唯一标识一个人，而URL可以告诉邮递员怎么把货送到你手里。

**再举个栗子：**

> http://blog.csdn.net/koflance是个URL，通过这个网址可以告诉CDN找到我的博客所在地，并且还告诉用HTTP协议访问，而isbn:0-395-36341-1是RUN，一个国际标准书号，可以唯一确定哪本书。

目前HTTP规范已经不使用URL，而是使用URI了，所以大家还是用URI吧，准没错！

> HTTP relies upon the Uniform Resource Identifier (URI) standard
> [RFC3986] to indicate the target resource (Section 5.1) and
> relationships between resources.

##### Java中的URI和URL组件

**URI组件**

该类是不可变类(Instances of this class are immutable)，提供了用于从其组成部分或通过解析其字符串形式创建 URI 实例的构造方法、用于访问实例的各个不同组成部分的方法，以及用于对 URI 实例进行规范化、解析和相对化的方法。

在java的uri类中，将URI字符串分为绝对RUI和相对URI，或者不透明URI(opaque uri)和分层URI(hierarchical uri)。其中，不透明 URI 为绝对 URI，且不透明的URI无法解析，例如：mailto:java-net@java.sun.com 、news:comp.lang.java、urn:isbn:096139210x，而分层URI即可以为绝对URI，也可以是相对URI，例如http://java.sun.com/j2se/1.3/ 是绝对URI，而../../../demo/jfc/SwingSet2/src/SwingSet2.java是相对URI。



在给定实例中，任何特殊组成部分都或者为未定义的，或者为已定义的，并且有不同的值。未定义的字符串组成部分由 null 表示，未定义的整数组成部分由 -1 表示。

常用方法

```java
URI normalize()    //规范化此URI的路径。(规范化: 将分层 URI 的路径组成部分中的不必要的 "." 和 ".." 部分移除的过程。每个 "." 部分都将被移除。".." 部分也被移除，除非它前面有一个非 ".." 部分。规范化对不透明 URI 不产生任何效果。) 
URI relativize(URI uri)    //根据此 URI 将给定 URI 相对化。 
URI parseServerAuthority()    //尝试将此 URI 的授权组成部分（如果已定义）解析为用户信息、主机和端口组成部分。 
URI resolve(URI uri)    //根据此 URI 解析给定的 URI。 
URI resolve(String str)    //解析给定的字符串，然后在此 URI 的基础上构造一个新的 URI。 此方法与进行 resolve(URI.create(str)) 的作用相同。12345
```

**URL组件**

提供了解析URL的功能，可以将URL解析成一个结构化的数据，并提供了简单的查找主机和打开到指定资源的连接之类的网络 I/O 操作。

第一个示例：

```java
URL url = new URL("http://www.baidu.com");
//打开到此 URL 的连接并返回一个用于从该连接读入的 InputStream。
InputStream in = url.openStream();// 其内部也调用了openConnection()
ByteArrayOutputStream output = new ByteArrayOutputStream();
byte[] buffer = new byte[1024];
int len = -1;
// 读取内容不含响应头
while ((len = in.read(buffer)) != -1)
{
   output.write(buffer, 0, len);
}
System.err.println(new String(output.toByteArray()));123456789101112
```

第二个示例：

```java
// 方法一 
URL url = new URL("http://www.sina.com.cn");
URLConnection urlcon = url.openConnection();
InputStream is = urlcon.getInputStream();

// 方法二
URL url = new URL("http://www.yhfund.com.cn");
HttpURLConnection urlcon = (HttpURLConnection)url.openConnection();
InputStream is = urlcon.getInputStream();

//方法三
URL url = new URL("http://www.yhfund.com.cn");
InputStream is = url.openStream();
```