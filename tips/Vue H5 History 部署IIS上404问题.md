# Vue H5 History 部署IIS上404问题

## 背景简介

> `vue`使用`vue-router`时，默认的地址并不美观，以`#`进行分割，例如：`http://www.xxx.com/#/main`。
> 为了访问地址能像正常的`url`一样，例如：`http://www.xxx.com/user/id`。
> 按照官网介绍，使用 history 模式。但是却产生了问题。



## 问题

> 因为我们的应用是单页客户端应用，当用户在浏览器直接访问`http://www.xxx.com/user/id`时，刷新页面的时候，会返回404错误。



## 问题原因

> 服务端URL匹配不到相应的路由资源



## 解决方案

> 官网提供的解决方案只支持Apache服务器以及Nginx服务器配置，然而IIS的解决方案并没有给出

- 方案一

  > 可通过给IIS站点设置虚拟目录的方式可解决该问题，但是这方式路由比较多的时候比较麻烦。
  > ![添加虚拟目录](http://images2015.cnblogs.com/blog/534030/201705/534030-20170506123459211-982428348.png)

- 方案二

  > 1、下载Web平台安装程序（https://www.microsoft.com/web/downloads/）
  > 2、如果已经安装过Web平台安装程序，可以在IIS站点看到该程序
  > ![查看Web平台安装程序](http://images2015.cnblogs.com/blog/534030/201705/534030-20170506123528773-680741688.png)
  > 3、查找`Url重写工具2.0`并进行安装
  > ![查找Url重写工具2.0](http://images2015.cnblogs.com/blog/534030/201705/534030-20170506123601414-1844667571.png)
  > 4、安装完毕后，重新打开IIS控制台，进入相应站点，就可以看到`URL重写`该功能模块
  > ![查看URL重写工具](http://images2015.cnblogs.com/blog/534030/201705/534030-20170506123613367-1740051765.png)
  > 5、添加规则，并选择`入站规则-空白规则`
  > ![添加规则](http://images2015.cnblogs.com/blog/534030/201705/534030-20170506123623632-1587679024.png)
  > ![完成规则](http://images2015.cnblogs.com/blog/534030/201705/534030-20170506123627273-2071549205.png)



## 总结

> Url重写设置
> 匹配的URL：请求的URL选择`与模式匹配`，模式中填写`*`，使用选项选择`通配符`;即表示所有的网站都通过此模式进行检查匹配。
> 条件：是下面的条件选项，我们选择`不是文件`，逻辑分组为全部匹配。
> 操作：重写到`index.html`（根据情况，设置为自己的单页面应用首页）。

以上操作是设置我们的页面请求为先检查有没有该文件，没有该文件全部重写到首页，从而能够使用自定义路由。然后在vue程序中设置`/index.html`路径为起始页，并且定义404页面。