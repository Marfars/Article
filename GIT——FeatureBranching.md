# Feature Branching

- [Feature Branching](#feature-branching)
  - [两个人协作的场景](#两个人协作的场景)
  - [什么是FeatureBranching](#什么是featurebranching)
  - [Git常用指令及其本质](#git常用指令及其本质)
    - [checkout](#checkout)
    - [rebase](#rebase)
  - [.git仓库](#git仓库)
    - [HEAD](#head)

## 两个人协作的场景

当我们提交之前，要养成一个好的习惯：**一定要先去拉一下代码**

## 什么是FeatureBranching

基于feature（特性）来开分支，就是每开发一个功能或者修复一个问题，都在原分支基础上开一个新的分支，保证了开发流程的灵活性和独立性

将feature分支合并至原分支（develop）的方式一般会有两种，一种是本地合并push到远端，另一种是push自己的提交至远端然后再合并。但是不管是哪一种，为了确保不会产生冲突，都要先pull一下，避免因为其他同事的修改而产生问题

## Git常用指令及其本质

### checkout

checkout翻译为中文是查看，签出，在Git中它所承担的角色为```切换HEAD的位置```。```HEAD```的含义我们在上篇文章中提到了，它是一个永远指向当前commit的引用，当我们想要切换它的位置，实际上就是希望将自己当前的代码版本切换至另一个commit代表的代码版本。这个指令很常见，常用在切换分支中。比如当前我们在develop分支，我们希望切换至master分支进行代码合并，我们就可以使用指令：```git checkout master```。

**不是切换至某个Commit吗？为什么会被常用来切换分支**？如果产生这个疑问的话，我们需要先回顾下之前对分支的本质定义。分支实际上就是一个引用，一个指针，它所指向的一定是某个commit。只不过我们很难一直去记住某个分支最新commit的唯一标识（Hash值），所以我们就使用这个分支引用来指代这个分支的最新Commit。master所代表的就是该分支上的最新commit（假设是commit13），那么我们checkout至这个分支，实际上就是将```HEAD```移动至了commit13，代码版本也会由之前的版本回溯至commit13的代码版本

主要注意的是

- 虽然checkout常被用来切换分支，但它不只是可以做这么多。如果我们希望切换至某个commit的代码版本（假设该commit哈希值为5d882282）,我们可以使用```git checkout 5d882282```，来将```HEAD```移动至指定位置

### rebase

rebase中文翻译为变基

## .git仓库

在项目根目录下有个隐藏文件夹.git，这个文件夹就代表了我们的本地仓库。其中有几个内容需要我们大致了解下，方便之后解决问题

### HEAD

这个文件代表了```HEAD```引用指向的内容。如果内容是一个引用，比如```refs/heads/master```，代表其指向了master分支的最新commit；如果内容是一个hash值，代表```HEAD```引用目前指向的是一个具体的commit