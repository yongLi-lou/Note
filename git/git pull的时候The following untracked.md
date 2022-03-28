# git pull的时候The following untracked working tree files would be overwritten by merge balabala...
解决办法：
git clean -d -fx

备注：会删除掉没有add到仓库的文件，操作记得慎重，以免改动文件的丢失。本质上就是操作仓库中没有被追踪的本地文件
``` git
$ git clean -f -n         # 1
$ git clean -f            # 2
$ git clean -fd           # 3
$ git clean -fX           # 4
$ git clean -fx           # 5
```
(1): 选项-n将显示执行（2）时将会移除哪些文件。
(2): 该命令会移除所有命令（1）中显示的文件。
(3): 如果你还想移除文件件，请使用选项-d。
(4): 如果你只想移除已被忽略的文件，请使用选项-X。
(5): 如果你想移除已被忽略和未被忽略的文件，请使用选项-x。