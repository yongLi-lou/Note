# postman的身份验证Authentication

1、概述
  Authorization是验证是否拥有从服务器访问所需数据的权限。当发送请求时，通常必须包含参数，以确保请求具有访问和返回所需数据的权限。Postman提供了授权类型，可以轻松地在Postman本地应用程序中处理身份验证协议。

 ![img](https://img2018.cnblogs.com/blog/1506513/201810/1506513-20181017114443196-498659577.png)

 

应当注意:NTLM和BearerToken仅在Postman本地应用程序中可用。所有其他授权类型都可以在Postman本地应用程序和Chrome应用程序中使用。

身份验证Authentication 

1、Inherit auth from parent （从父类继承身份验证）

向集合或文件夹添加授权。
 假设您在集合中添加了一个文件夹。在授权选项卡下，默认的授权类型将被设置为“从父类继承auth”。

“从父”设置的“继承auth”指示默认情况下，该文件夹中的每个请求都使用父类的授权类型。

2、No Auth

默认情况下，“No Auth”出现在下拉菜单列表中。当您不需要授权参数发送请求时，使用“No Auth”。

3、Basic Auth

是基础的验证，所以会比较简单 
会直接把用户名、密码的信息放在请求的 Header 中

4、Digest Auth

要比Basic Auth复杂的多。使用当前填写的值生成authorization header。所以在生成header之前要确保设置的正确性。如果当前的header已经存在，postman会移除之前的header。

5、OAuth 1.0

postman的OAuth helper让你签署支持OAuth

1.0基于身份验证的请求。OAuth不用获取access token,你需要去API提供者获取的。OAuth 1.0可以在header或者查询参数中设置value。

6、OAuth 2.0

postman支持获得OAuth 2.0 token并添加到requests中。

7、Bearer Token

Bearer Token是安全令牌。任何带有Bearer Token的用户都可以使用它来访问数据资源，而无需使用加密密钥。