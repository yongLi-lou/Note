# git解决冲突与merge

git111冲突的场景与其他SCM工具一样，我在这边修改了文件a，同事也修改了文件a。同事比我先提交到仓库中，那么我pull代码时就会报错：

```
$ git pull
remote: Counting objects: 39, done.
remote: Compressing objects: 100% (30/30), done.
remote: Total 39 (delta 13), reused 0 (delta 0)
Unpacking objects: 100% (39/39), done.
From https://code.csdn.net/lincyang/androidwidgetdemo
   d3b2814..5578b8c  master     -> origin/master
Updating d3b2814..5578b8c
error: Your local changes to the following files would be overwritten by merge:
    app/src/main/AndroidManifest.xml
    app/src/main/java/com/linc/skill/screenswitch/ScreenSwichActivity.java
Please, commit your changes or stash them before you can merge.
Aborting
```

而此时我又不顾这个错误，将我的代码add并commit，然后push时报如下错：

```
To https://code.csdn.net/lincyang/androidwidgetdemo.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://code.csdn.net/lincyang/androidwidgetdemo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

然后我有执行了git pull：

```
$ git pull
Auto-merging app/src/main/java/com/linc/skill/screenswitch/ScreenSwichActivity.java
CONFLICT (content): Merge conflict in app/src/main/java/com/linc/skill/screenswitch/ScreenSwichActivity.java
Auto-merging app/src/main/AndroidManifest.xml
CONFLICT (content): Merge conflict in app/src/main/AndroidManifest.xml
Automatic merge failed; fix conflicts and then commit the result.
```

那么既然两个文件冲突，我就可以借助mergetool来搞定它。我安装了meld作为代码比对工具，那么它理所当然也就成为我的mergetool了。

```
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
meld opendiff kdiff3 tkdiff xxdiff tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc3 codecompare emerge vimdiff
Merging:
app/src/main/AndroidManifest.xml
app/src/main/java/com/linc/skill/screenswitch/ScreenSwichActivity.java

Normal merge conflict for 'app/src/main/AndroidManifest.xml':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (meld): 

Normal merge conflict for 'app/src/main/java/com/linc/skill/screenswitch/ScreenSwichActivity.java':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (meld): 
```

至此，本次冲突解决完毕。

如果希望保留生产服务器上所做的改动,仅仅并入新配置项, 处理方法如下:

```
git stash
git pull
git stash pop
```

