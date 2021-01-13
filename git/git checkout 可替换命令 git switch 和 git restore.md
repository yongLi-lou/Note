# git checkout 可替换命令 git switch 和 git restore

git checkout 这个命令承担了太多职责，既被用来切换分支，又被用来恢复工作区文件，对用户造成了很大的认知负担。

Git社区发布了Git的新版本2.23。在该版本中，有一个特性非常引人瞩目，就是新版本的Git引入了两个新命令 git switch 和 git restore，用以替代现在的 git checkout。换言之，git checkout 将逐渐退出历史舞台。

Git社区决定这样做，是因为目前 git checkout 命令承载了太多的功能，这让新手们感到困惑。git checkout 的核心功能包括两个方面，一个是分支的管理，一个是文件的恢复。这两个核心功能，未来将由 git switch 和 git restore 分别负责。

相比之下，新命令旨在将职责明确分为两个较窄的类别：更改分支的操作和更改文件的操作。为此，请照顾前者和后者。为了我们的目的，让我们更详细地了解每个。`git checkout``git switch``git restore`

***\**\*\*\*案例比较\*\*\*\*\****

**1、分支管理**

在分支管理方面，目前切换分支以及创建分支，是使用

```
git checkout <分支名> 和 git checkout -b <分支名>
```

有了 git switch 之后，对应的命令分别为

```
git switch <分支名> 和 git switch -c <分支名>　
```

最简单的调用如下所示：`git switch`

```
$ git switch my-feature

Switched to branch 'my-feature'

Your branch is up to date with 'origin/my-feature'
```

在这种模式下，您可以将其视为类似于的无选择调用。如果要替换（或简称，），可以编写：`git switch``git checkout``git checkout --branch``git checkout -b`

创建并且切换新分支

```
Tinywan@DESKTOP-TEDEO2S MINGW64 /d//wiot.tinywan.com (master)
$ git switch -c my-new-feature
Switched to a new branch 'my-new-feature'

Tinywan@DESKTOP-TEDEO2S MINGW64 /d/wiot.tinywan.com (my-new-feature)
$ git branch
```

这里是短期的，并可以取代你的肌肉记忆。或者，从指定的提交开始（而不是分支当前已检出的提交）：`-c``--create``git checkout -b`　

```
$ git switch -c my-new-feature 0810beaed7

Switched to a new branch 'my-new-feature' 
```

该[文档中](https://git-scm.com/docs/git-switch/2.23.0)还有其他一些示例，包括上述更多示例以及有关如何设置跟踪分支，孤立分支等的一些示例。

**2、文件恢复**

```
git restore`另一方面，它使确定要更改的文件，要更改的文件以及更改的位置变得容易得多。除了记住的纠结语义外，还提供了两个选项来指定还原的更改将移至何处。如果您通过（或完全不通过任何操作），则更改将进入您的工作副本。如果通过，它们将进入您的索引。最后，如果您同时通过了两者，则两者都将更改为相同的内容。`git checkout``git restore``--worktree``--staged
git restore`还可以通过可选选项更轻松地了解要还原的内容来自何处。如果未指定，则从登台区域还原内容，否则，从特定树还原它们。注意：和之间的另一个区别是，默认为和将删除源树中不存在的跟踪文件。请参阅我们的[2.22博客文章中](https://github.blog/2019-06-07-highlights-from-git-2-22/)的讨论。 `--source``--source``git checkout``git restore``restore``--no-overlay``--overlay
```

例如，如果我希望文件在工作副本和索引中的内容都与三次提交之前相同，则可以这样写：`main.c`

```
$ git restore --source HEAD~3 --staged --worktree main.c
```

……更改将完全适用。的[新文档中`git restore`](https://git-scm.com/docs/git-restore/2.23.0)还有更多示例。

 **小结**

该文档中还有其他一些示例，包括上述更多示例以及有关如何设置跟踪分支，孤立分支等的一些示例。
git restore另一方面，它使确定要更改的文件，要更改的文件以及更改的位置变得容易得多。除了记住的纠结语义外，还提供了两个选项来指定还原的更改将移至何处。如果您通过（或完全不通过任何操作），则更改将进入您的工作副本。如果通过，它们将进入您的索引。最后，如果您同时通过了两者，则两者都将更改为相同的内容。

```
git checkoutgit restore--worktree--staged
```