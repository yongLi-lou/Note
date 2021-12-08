# 微信开发-业务域名、JS接口安全域名、网页授权域名

在微信公众平台上可配置这些域名。

　　![img](https://img2018.cnblogs.com/blog/596671/201810/596671-20181015173339318-1745837623.png)

 

1.业务域名：在微信浏览器中点击文本框，会弹出下面的提示，很不爽，通过配置业务域名可以将该提示去掉

　　![img](https://img2018.cnblogs.com/blog/596671/201810/596671-20181015173357318-29412687.png)

 

2.JS接口安全域名：分享到朋友圈（js-sdk）时用上，此接口要求将当前的界面url加密后，才可以分享到朋友圈。
采用前后端分离开发时，js-sdk的验证参数通过php接口获得时，会报invalid signature错误。解决方法：前端将当前的window.location.href传到php接口，php代码中将下图中的$url换成前端传过来的url，生成验证相应参数，再返回，在生成分享链接时才不会出错

![img](https://img2018.cnblogs.com/blog/596671/201810/596671-20181015173421975-1465115250.png)

 

3.网页授权域名：用于获取用户针对于公众号的唯一标识openid。但只能添加一个域名。我设置为一级域名后，同一服务器上，通过二级域名访问的就不能通过网页授权了。我的解决方法是，将网页授权的redirect_uri设置为php接口，这样，用户点击https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx4e2480691f223ba7&redirect_uri=http://xxx/xxx.php &response_type=code&scope=snsapi_base&state=1#wechat_redirect
。在php接口中拿到code，调微信的接口，换取openid，再跳转回前端界面，同时把openid带回去。