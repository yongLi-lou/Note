# git pull request 流程

git pull request 用于在 fork 官方 repo 到个人 github, 在本地修改后，向官方 repo 请求合并。在官方团队审查过代码后，就可以将自己所做的改动合并到官方 repo 的 branch。

## 流程

- fork
  在 github 上 fork 别人的 repo 到自己 github.
- clone
  clone 到本地后开新的 branch 做修改
- commit
  修改后推送到自己的 github 的 new branch 上
- pull request
  在 github 上发起 pull request, 选择要请求合并的分支
- discuss and review
  repo 的团队审查代码。
- merge
  repo 的团队审查通过后会合并到官方的 repo 的 master 或其他 branch 中，然后关闭这个 pull request.

 

 

```
git add -A
git commit -m "注释"
git remote -v 查看远程端名称 "git remote -help 查看git remote所有操作指令"
git push 远程库名字origin 本地库名字master
```