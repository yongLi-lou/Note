# web service

什么是web service？

   答：soap请求是HTTP POST的一个专用版本，遵循一种特殊的xml消息格式Content-type设置为: text/xml任何数据都可以xml化。

为什么要学习web service？

​    答：大多数对外接口会实现web service方法而不是http方法，如果你不会，那就没有办法对接。

web service相对http (post/get)有好处吗？

​      1.接口中实现的方法和要求参数一目了然

​       2.不用担心大小写问题 

​      3.不用担心中文urlencode问题

​      4.代码中不用多次声明认证(账号,密码)参数

​      5.传递参数可以为数组，对象等...

web service相对http（post/get）快吗？

   答：由于要进行xml解析，速度可能会有所降低。

web service 可以被http（post/get）替代吗？

​    答：完全可以，而且现在的开放平台都是用的HTTP（post/get）实现的。

httpservice通过post和get得到你想要的东西

webservice就是使用soap协议得到你想要的东西，相比httpservice能处理些更加复杂的数据类型

http协议传输的都是字符串了，webservice则是包装成了更复杂的对象。

1. webservice走HTTP协议和80端口。

2. 而你说的api，用的协议和端口，是根据开发人员定义的。

3. 这么说吧，api类似于cs架构，需要同时开发客户端API和服务器端程序。

4. 而WebService则类似于bs架构，只需要开发服务器端，不需要开发客户端，客户端只要遵循soap协议，就可以调用。

java用webservice作接口有什么好处？直接提供一个请求地址就行了啊

 答：对开发语言通用类型做xml映射处理，跨语言，跨平台。

### 如何使用.net 编写webservice 

1. 创建 web引用程序
2. 添加新建项Web 服务（ASMX）
3. 发布

### 如何使用.net core调用webservice 

1. 连接到 wcf web service
2. 配置
3. 注册服务
4. 依赖注入
5. 使用xxrequestbody和xxresponsebody模型
6. 使用service的方法