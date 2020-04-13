# Git from the bottom up | 自底向上聊 Git

这是一个 [git from the bottom up](https://jwiegley.github.io/git-from-the-bottom-up/)的中文翻译.

由于译者水平有限, 保留了英文, 防止误解.

## Introduction | 介绍

> Welcome to the world of Git. I hope this document will help to advance your understanding of this powerful content tracking system, and reveal a bit of the simplicity underlying it — however dizzying its array of options may seem from the outside.

欢迎来到 Git 的世界. 我希望这个文档可以加深读者对这一个内容跟踪系统的认识. 虽然 Git 的操作选项从使用者的角度看起来令人头晕眼花, 但是我希望借这篇文档展示它底层的简洁性.

> Before we dive in, there are a few terms which should be mentioned first, since they’ll appear repeatedly throughout this text:

在我们进一步讨论之前, 有几个名词我们需要先做出一些解释, 之后我们将在这篇文章中不断地见到他们.

### repository

> repository — A repository is a collection of commits, each of which is an archive of what the project’s working tree looked like at a past date, whether on your machine or someone else’s. It also defines HEAD (see below), which identifies the branch or commit the current working tree stemmed from. Lastly, it contains a set of branches and tags, to identify certain commits by name.

repository - repository 是由若干 commit 组成的集合, 这些 commit 可以存在你的机器上, 也可以在其他的地方, 它们是 working tree 曾经状态的存档. repository 同时定义了 HEAD (下面有), HEAD 标志着当前的 working tree 的来源. repository 还含有一个由 branch 和 tags 组成的集合, branch 和 tags 可以看作是某个 commit 的别名, 用户可以通过这个别名来找到某个 commit. 

### the index

> the index — Unlike other, similar tools you may have used, Git does not commit changes directly from the working tree into the repository. Instead, changes are first registered in something called the index. Think of it as a way of “confirming” your changes, one by one, before doing a commit (which records all your approved changes at once). Some find it helpful to call it instead as the “staging area”, instead of the index.

the index - Git 并不像其他你使用过的类似工具那样直接把 working tree 中的更改直接 commit 到repository 中, 而是先将 working tree 中的更改注册到一个叫 the index 的地方. 你可以把这看作是 Git 要求你在做一个 commit 之前, 对你的每个更改的再次确认, 这个 commit 将会一次性记录所有你再次确认过的更改. 有人认为与其叫它 the index 不如将其称作 staging area (暂存区).

### working tree

> working tree — A working tree is any directory on your filesystem which has a repository associated with it (typically indicated by the presence of a sub-directory within it named .git.). It includes all the files and sub-directories in that directory.

working tree - working tree 是在你的文件系统中任何一个与某个 repository 相关联的文件夹, 通常来说这样的文件夹都会有一个叫 .git 的子目录. 一个 working tree 包含了这样一个文件夹下所有的文件和子目录.

### commit

> commit — A commit is a snapshot of your working tree at some point in time. The state of HEAD (see below) at the time your commit is made becomes that commit’s parent. This is what creates the notion of a “revision history”.

commit - commit 是某个时间点上你的 working tree 的一个快照.[^1] 当你提交一个 commit 的时候, HEAD 指向的 commit 会成为你提交的 commit 的父 commit. 这个机制让我们得以追溯修改的历史.

### branch

> branch — A branch is just a name for a commit (and much more will be said about commits in a moment), also called a reference. It’s the parentage of a commit which defines its history, and thus the typical notion of a “branch of development”.

branch - branch 只是某个的 commit 的别名[^2], 也可以认为是一个引用. 它是一个 commit 的来历, 记录着我们是如何到达这个 commit 的. 这很好地体现了分支开发的理念.

### tag

> tag — A tag is also a name for a commit, similar to a branch, except that it always names the same commit, and can have its own description text.

tag - tag 是另一种 commit 的别名, 和 branch 很类似, 除了它永远都是某个特定 commit 的别名, 以及一个 tag 可以有它自己的说明文字.

### master

> master — The mainline of development in most repositories is done on a branch called “**master**”. Although this is a typical default, it is in no way special.

master - 大部分主要的开发都在一个叫 master 的分支上完成, 这只是一个默认的名称, 并没有什么特殊的.

### HEAD

> HEAD — HEAD is used by your repository to define what is currently checked out:

HEAD - HEAD 是被你的 repository 用来确定什么东西是当前 checkout 的.

> If you checkout a branch, HEAD symbolically refers to that branch, indicating that the branch name should be updated after the next commit operation.

如果你 checkout 一个 branch, HEAD 会象征性地[^3]指向那个 branch, 这意味着这个 branch 在下一次 commit 之后会更新.  

> If you checkout a specific commit, HEAD refers to that commit only. This is referred to as a detached HEAD, and occurs, for example, if you check out a tag name.

如果你 checkout 某个特定的 commit, 那么 HEAD 只会指向那个 commit. 发生这种事情的时候, 我们称一个 HEAD 处在脱离的状态. 当你 checkout 一个 tag 的时候也会发生这样的事情.



> The usual flow of events is this: After creating a repository, your work is done in the working tree. Once your work reaches a significant point — the completion of a bug, the end of the working day, a moment when everything compiles — you add your changes successively to the index. Once the index contains everything you intend to commit, you record its content in the repository. Here’s a simple diagram that shows a typical project’s life-cycle:

通常来说你用 Git 工作的流程是这样的: 在创建一个 repository 之后, 你在 working tree 中完成工作. 每当你完成了一个阶段的工作, 比方说修正了一个 bug, 或者下班了, 还有比如"终于能过编译了!",你就把所有的更改都添加到 the index 中. 当 the index 中包含了所有你想 commit 的内容, 你就将这个 commit 的内容加入到 repository 中.这里有一个简单的示意图,展示了一个项目通常的生命周期.

![Project Lifecycle](https://jwiegley.github.io/git-from-the-bottom-up/images/lifecycle.png)

> With this basic picture in mind, the following sections shall attempt to describe how each of these different entities is important to the operation of Git.

请将上述的内容记在心里, 接下来的几节我们将说明这些东西对 Git 的操作的重要性.

[^1]: 这个描述貌似和上面对 working tree 的描述有冲突, 上面对 working tree 的说法是 working tree 包含了 .git 子目录,但是出于译者对 commit 的认识而言, commit 中应该没有 .git 中的相关信息. 这里可以尝试理解为文中说的 working tree 是类似工作区的概念, 并不包括 .git 子目录

[^2]: 这个地方括号中的内容不知道怎么翻译, 可以理解为, "我们还有很多关于 commit 的东西马上就要谈到", 但是感觉和上下文不搭.

## Repository: Directory content tracking | Repository: 目录内容跟踪

> As mentioned above, what Git does is quite rudimentary: it maintains snapshots of a directory’s contents. Much of its internal design can be understood in terms of this basic task.

就像上面提到的一样, Git 实际上做的事情非常的基础: 只是在维护一个目录里的内容的快照. Git 的许多内部设计都可以从这个角度来理解.

> The design of a Git repository in many ways mirrors the structure of a UNIX filesystem:

Git 的 repository 的设计很多时候都和 UNIX 的文件系统很相似.

> A filesystem begins with a root directory, which typically consists of other directories, most of which have leaf nodes, or files, that contain data. Meta-data about these files’ contents is stored both in the directory (the names), and in the i-nodes that reference the contents of those files (their size, type, permissions, etc). Each i-node has a unique number that identifies the contents of its related file. And while you may have many directory entries pointing to a particular i-node (i.e., hard-links), it’s the i-node which “owns” the contents stored on your filesystem.



Internally, Git shares a strikingly similar structure, albeit with one or two key differences. First, it represents your file’s contents in blobs, which are also leaf nodes in something awfully close to a directory, called a tree. Just as an i-node is uniquely identified by a system-assigned number, a blob is named by computing the SHA1 hash id of its size and contents. For all intents and purposes this is just an arbitrary number, like an i-node, except that it has two additional properties: first, it verifies the blob’s contents will never change; and second, the same contents shall always be represented by the same blob, no matter where it appears: across commits, across repositories — even across the whole Internet. If multiple trees reference the same blob, this is just like hard-linking: the blob will not disappear from your repository as long as there is at least one link remaining to it.

The difference between a Git blob and a filesystem’s file is that a blob stores no metadata about its content. All such information is kept in the tree that holds the blob. One tree may know those contents as a file named “foo” that was created in August 2004, while another tree may know the same contents as a file named “bar” that was created five years later. In a normal filesystem, two files with the same contents but with such different metadata would always be represented as two independent files. Why this difference? Mainly, it’s because a filesystem is designed to support files that change, whereas Git is not. The fact that data is immutable in the Git repository is what makes all of this work and so a different design was needed. And as it turns out, this design allows for much more compact storage, since all objects having identical content can be shared, no matter where they are.
