# GIT核心概念：一切皆引用

- [GIT核心概念：一切皆引用](#git核心概念一切皆引用)
  - [使用方法](#使用方法)
    - [```git clone```](#git-clone)
    - [```git log```](#git-log)
    - [```git add```](#git-add)
    - [```git commit```(补充一些图)](#git-commit补充一些图)
    - [```git push```](#git-push)
    - [```git fetch```](#git-fetch)
    - [```git merge```](#git-merge)
  - [细说Commit](#细说commit)
    - [Commit是什么(补充commit hash值的算法)](#commit是什么补充commit-hash值的算法)
    - [暂存区Staging Area](#暂存区staging-area)

## 使用方法

在正式开始这个部分之前，我们先简单回顾下Git基本的使用方法

- 命令行和可视化工具并没有高下之分，一个工具而已，哪个顺手用哪个。只要能把东西做好，哪种方法都行。既然有了方便快捷的方式，为什么不用呢？

### ```git clone```

todo 补充一个仓库地址

- 这个命令做到的不只是简单把文件复制下来这么简单，它将远端的整个仓库，包括之前commit、branch、tag全部都拉取了下来

- 假设一种场景，同事将一个文件a.txt删掉了，并且合并到了远端分支。我们在拉取同事的这个commit之后，本地的a.txt也会一并消失。但是这个文件并不是真的彻底消失了，只是针对当前的这个节点，它处于被删除的状态，我们仍旧可以通过git指令查看历史将文件还原回来

- 分布式VCS和中央式VCS比较大的一个区别就是，分布式可以脱离远程仓库去做完整的版本管理

### ```git log```

拉取到代码之后，我们可以执行一下这个指令来看一下commit日志记录

- 这个指令的展示结果取决于当前处于哪个分支，在develop分支下执行这个指令就只会展示develop分支的commit日志记录

- 如果我们在develop分支下想要查看master分支的commit日志记录，可以使用```git log master```

- 这个指令其实可以做很多事，比如查看哪个节点到哪个节点的代码变更情况，查看某位同事的所有提交记录等等，可以通过附加参数的方式实现。此处不再赘述，如有需要可以自行至git官网查阅文档

### ```git add```

如果我们想将App.js这个文件提交到**暂存区（后面会对其进行解释，目前就理解为commit文件的待选区吧）**，我们可以执行```git add App.js```指令将此文件提交至暂存区

- 实际工作中，如果使用命令行提交文件，我们更多的是使用```git add .```，将变更的所有文件都提交至暂存区，因为我们不太可能一个文件变一次提交一次，也不太可能修改结束后将所有的文件挨个执行```git add```

- ```git add .```指令执行的前提最好是项目有一个比较完善的.gitignore文件，要不然会提交很多的无用文件。.gitignore文件是我们在提交至暂存区的时候告诉git，我的哪些文件发生变更后不需要提交，常见的是一些本地配置类文件和运行时生成文件

- 需要注意的是，.gitignore只会忽略没有被添加到暂存区的文件，因为git存在本地缓存，如果文件已经纳入了版本管理，此时修改.gitignore文件是无效的

### ```git commit```(补充一些图)

刚刚我们使用add指令添加了App.js到暂存区，现在我们感觉本次修改这些就足够了，那就可以将暂存区的文件包装一下形成一个commit，提交到我们**本地**的仓库：```git commit```，执行执行后会出现vim的编辑页面，我们可以按照vim的操作方式填写自己的commit信息：```feat：something```

- 这个指令会将我们暂存区里的所有文件整合至一个commit中，目前是只有App.js，但如果暂存区还存在Home.js或者其他文件，也会被一次性整合到刚才的commit中

- 如果不想进入vim页面编辑，可以使用如下指令： ```git commit -m "feature: something"```

在执行完commit之后，如果我们再执行```git log```指令的话，就会发现现在这个分支最上面的commit信息就是我们刚刚输入的内容，这样我们就使用命令行完成了一次**本地提交**，请注意，如果此时去同事花哥那边是看不到这个commit信息的。因为我们只是提交到了自己的本地仓库，并没有合并至远端仓库

### ```git push```

如果我们认为这个commit的任务已经完成了，那我们就可以将它推送到远端，让远程仓库也拥有这部分变更代码和记录：```git push```，执行此结果后，远端仓库就会有对应的响应，比如推送到github就会出现对应的commit，推送到gerrit就会开始code review流程

- 移动业务组的小伙伴，如果你尝试直接使用```git push```将commit推送到gerrit，你会发现报错```! [remote rejected] develop -> develop (prohibited by Gerrit)```，这个是因为Gerrit自身的一些限制。此时如果我们希望推送至远端，可以使用```git push origin refs/heads/develop:refs/for/develop```

### ```git fetch```

代码合并至远端仓库之后，同事花哥就可以使用```git fetch```指令，将远端我们刚刚提交的```feature: something```拉取到他的本地。但是花哥在develop分支会发现此时他的文件目录中并没有App.js文件，这并不是此次提交没有被拉取到花哥本地。而是被放置到了```origin/develop```分支

- origin/develop是我们本地对远端develop做的快照，我们一直在开发的develop分支一定是比origin/develop新的。我们是没有办法向本地的origin/develop分支提交代码的，这个分支的变更来源一定是```git fetch```拉取到的远端提交

### ```git merge```

如果我们希望将origin/develop刚刚的那个提交拿到自己的develop分支，可以使用```git merge origin/develop```指令，这个会将两个分支合并至一起，将origin/develop比develop分支多的那个```feature: something```提交合并过来

- 如果担心merge会有冲突产生可以考虑使用```git rebase```（例子中的具体指令为```git rebase origin/develop```）。rebase和merge两者都旨在将变更代码从一个分支合并至另一个分支（例子中的从origin/develop到develop）

- 默认情况（不附加任何参数），```git fetch``` + ```git merge FETCH_HEAD```等价于```git pull```，也就是说例子中的整个拉取合并流程可以被```git pull origin develop```替代（HEAD字样后面会提到）

## 细说Commit

### Commit是什么(补充commit hash值的算法)

commit记录了每次改动，```git log```中每个commit后面的哈希值是其唯一标识。

### 暂存区Staging Area

我们执行```git add```操作，就是将希望提交的内容放置暂存区，当我们下一次执行```git commit```操作的时候，就是将暂存区里的所有内容整理为一个新的commit，提交至本地仓库

- 在我们根目录文件夹可以看到一个隐藏的.git文件夹，这个就是我们的本地仓库。我们可以在这里文件夹里看到所有的提交记录