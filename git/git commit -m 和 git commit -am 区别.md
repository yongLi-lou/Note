# git commit -m 和 git commit -am 区别

## 通常修改一个文件 并且将文件提交到本地分支的命令是：

```
git add .
git commit -m 'update'
```

## 以上两个命令其实可以合并一处使用（简化成）：

```
git commit -am 'update'
```

【am】 就是 【add modify】 两个命令的合并

## 如果项目添加了新文件，必须使用分开的命令，不能用合并命令

```
git add .
git commit -m 'update'
```

