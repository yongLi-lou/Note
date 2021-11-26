# git账号密码问题

git账号密码输错了，但是缓存一直存在，解决方法：

```shell
1.先清空缓存  git config --system --unset credential.helper

2.再次 git clone  url

3.重新输入账号,再输入密码


```

这样就可以解决问题了，但是会存在另外一个问题，就是每次push、pull的时候都需要输入密码，很是麻烦
解决方法：**git永久保存账号密码**

方法1：直接在git bash 中执行命令：**git config --global credential.helper store**

```shell
git config --global credential.helper store
```

然后再输入一次账号密码登录就可以保存了。

方法2：安装好git之后一般会在C盘的C:\Users\Administator目录下生成 **.gitconfig**配置文件。用文档编辑工具打开该文件

添加：

```
[user]
	name = xiaobai  //你的git用户名

	email = xiaobai@1.com  //你的git邮箱账号

[credential]
    helper = store


然后保存就可以了。
```

或者：

```git
git config --global user.name "你的用户名"

git config --global user.email 你的邮箱
```

