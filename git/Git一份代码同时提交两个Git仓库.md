# Git一份代码同时提交两个Git仓库

# 方法1：
## 查看Git仓库

首先查看Git代码绑定了哪些Git仓库

```shell
git remote -v
```

如果当前代码没有绑定远端Git仓库，需要先确定Pull会从哪个仓库Pull，之后运行如下命令

```shell
git remote add orgin https://gitee.com/enilu/material-admin.git
```

## 绑定多个远端仓库

之后再绑定另外一个远端仓库，使Push的时候能同时Push两个仓库

```shell
git remote set-url --add origin https://github.com/enilu/material-admin.git
```

这个时候查看远端仓库信息会有两个Push的远程仓库

```shell
$ git remote -v
orgin   https://github.com/MeterosBehind/MyJava.git (fetch)
orgin   https://github.com/MeterosBehind/MyJava.git (push)
orgin   https://gitee.com/youngforever1728/my-java.git (push)
```

# 方法2

## 修改 .git\config文件

```c#
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true
[remote "origin"]
	url = 地址
	fetch = +refs/heads/*:refs/remotes/origin/*
	url = 新地址 // ### 这里添加多一个新仓库地址 ###
```





# 删除仓库origin

```shell
git remote rm origin
```

