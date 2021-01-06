# git merge：合并到master

**前言**

一人开发时可以直接提交到master（个人开发也不推荐此方法），没有什么问题；但是多人开发时，直接提交到master，可能会死的很惨，所以合并分支到master成了一件重要且必不可少的事情；在多次吃亏后写下这个简单的git merge合并到master。

**说明**

博主的git上已经有代码。

**操作**

大致流程讲解：创建新分支，将自己写好的代码提交到该分支上，然后切回master分支，使用git merge 新分支名称 ,最后将本地合并之后的代码同步到master、

————————————

1、新建分支jet5fly,并切换到新分支

git branch jet5fly
 git checkout jet5fly

或

git checkout -b jet5fly

使用git branch，查看当前分支。

2、开发完提交到远程分支 **(修改代码之前一定要最先执行pull操作)**

git add .
 git commit -m "commit infomation"
 git push origin jet5fly

3、返回master

git checkout master

4、把本地分支合并到master

git merge jet5fly

5、把本地合并后的master同步到远程

git push origin master