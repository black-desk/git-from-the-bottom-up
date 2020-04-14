# Git from the bottom up | 自底向上聊 Git



这是一个 [git from the bottom up](https://jwiegley.github.io/git-from-the-bottom-up/) 的中文翻译.

由于译者水平有限, 保留了英文, 防止误解.





## Introduction | 介绍



> Welcome to the world of Git. I hope this document will help to advance your understanding of this powerful content tracking system, and reveal a bit of the simplicity underlying it — however dizzying its array of options may seem from the outside.

欢迎来到 Git 的世界. 我希望这个文档可以加深读者对这一个内容跟踪系统的认识. 虽然 Git 的操作选项从使用者的角度看起来令人头晕眼花, 但是我希望借这篇文档展示它底层的简洁性.

> Before we dive in, there are a few terms which should be mentioned first, since they’ll appear repeatedly throughout this text:

在我们进一步讨论之前, 有几个名词我们需要先做出一些解释, 之后我们将在这篇文章中不断地见到他们.



### repository



> * **repository** — A **repository** is a collection of _commits_, each of which is an archive of what the project's _working tree_ looked like at a past date, whether on your machine or someone else's. It also defines HEAD (see below), which identifies the branch or commit the current working tree stemmed from. Lastly, it contains a set of _branches_ and _tags_, to identify certain commits by name.

* **repository** — **repository** 是由若干 _commit_ 组成的集合, 这些 _commit_ 可以存在你的机器上, 也可以在其他的地方, 它们是 *working tree* 曾经状态的存档. **repository** 同时定义了 HEAD (下面有), 它标志着当前的 working tree 的来源. **repository** 还含有一个由 _branch_ 和 _tag_ 组成的集合, _branch_ 和 _tags_ 可以看作是某个 commit 的别名, 用户可以通过这个别名来找到某个 commit. 



### the index



> * **the index** — Unlike other, similar tools you may have used, Git does not commit changes directly from the _working tree_ into the _repository_. Instead, changes are first registered in something called **the index**. Think of it as a way of “confirming” your changes, one by one, before doing a commit (which records all your approved changes at once). Some find it helpful to call it instead as the “staging area”, instead of the index.

* **the index** — Git 并不像其他你使用过的类似工具那样直接把 _working tree_ 中的更改直接 commit 到 _repository_ 中, 而是先将 _working tree_ 中的更改注册到一个叫 **the index** 的地方. 你可以把这看作是 Git 要求你在做一个 commit 之前, 对你的每个更改的再次确认, 这个 commit 将会一次性记录所有你再次确认过的更改. 有人认为与其叫它 the index 不如将其称作 staging area (暂存区).



### working tree



> * **working tree** — A **working tree** is any directory on your filesystem which has a _repository_ associated with it (typically indicated by the presence of a sub-directory within it named `.git`.). It includes all the files and sub-directories in that directory.

* **working tree** — **working tree** 是在你的文件系统中任何一个与某个 _repository_ 相关联的文件夹, 通常来说这样的文件夹都会有一个叫 `.git` 的子目录. 一个 **working tree** 包含了这样一个文件夹下所有的文件和子目录.



### commit



> * **commit** — A **commit** is a snapshot of your working tree at some point in time. The state of HEAD (see below) at the time your commit is made becomes that commit’s parent. This is what creates the notion of a “revision history”.

* **commit** — **commit** 是某个时间点上你的 working tree 的一个快照[^1]. 当你提交一个 commit 的时候, HEAD 指向的 commit 会成为你提交的 commit 的父 commit. 这个机制让我们得以追溯修改的历史.



### branch



> * **branch** — A **branch** is just a name for a commit (and much more will be said about commits in a moment), also called a reference. It’s the parentage of a commit which defines its history, and thus the typical notion of a “branch of development”.

* **branch** — **branch** 只是某个的 commit 的别名[^2], 也可以认为是一个引用. 它是一个 commit 的来历, 记录着我们是如何到达这个 commit 的. 这很好地体现了分支开发的理念.



### tag



> * **tag** — A **tag** is also a name for a commit, similar to a _branch_, except that it always names the same commit, and can have its own description text.

* **tag** — **tag** 是另一种 commit 的别名, 和 _branch_ 很类似, 除了它永远都是某个特定 commit 的别名, 以及一个 tag 可以有它自己的说明文字.



### master



> * **master** — The mainline of development in most repositories is done on a branch called “**master**”. Although this is a typical default, it is in no way special.

* **master** — 大部分主要的开发都在一个叫 "**master**" 的分支上完成, 这只是一个默认的名称, 并没有什么特殊的.



### HEAD



> * **HEAD** — **HEAD** is used by your repository to define what is currently checked out:
    * If you checkout a branch, HEAD symbolically refers to that branch, indicating that the branch name should be updated after the next commit operation.
    *  If you checkout a specific commit, HEAD refers to that commit only. This is referred to as a detached _HEAD_, and occurs, for example, if you check out a tag name.

* **HEAD** — **HEAD** 是被你的 repository 用来确定什么东西是当前 checkout 的:
  * 如果你 checkout 一个 branch, HEAD 会象征性地[^3]指向那个 branch, 这意味着这个 branch 在下一次 commit 之后会更新.  
  * 如果你 checkout 某个特定的 commit, 那么 HEAD 只会指向那个 commit. 发生这种事情的时候, 我们称一个 HEAD 处在脱离的状态. 当你 checkout 一个 tag 的时候也会发生这样的事情.



> The usual flow of events is this: After creating a repository, your work is done in the working tree. Once your work reaches a significant point — the completion of a bug, the end of the working day, a moment when everything compiles — you add your changes successively to the index. Once the index contains everything you intend to commit, you record its content in the repository. Here’s a simple diagram that shows a typical project’s life-cycle:

通常来说, 你用 Git 工作的流程是这样的: 在创建一个 repository 之后, 你在 working tree 中完成工作. 每当你完成了一个阶段的工作, 比方说修正了一个 bug, 或者下班了, 还有比如"终于能过编译了!",你就把所有的更改都添加到 the index 中. 当 the index 中包含了所有你想 commit 的内容, 你就将这个 commit 的内容加入到 repository 中.这里有一个简单的示意图,展示了一个项目通常的生命周期:



![Project Lifecycle](https://jwiegley.github.io/git-from-the-bottom-up/images/lifecycle.png)



> With this basic picture in mind, the following sections shall attempt to describe how each of these different entities is important to the operation of Git.

请将上述的内容记在心里, 接下来的几节我们将说明这些东西对 Git 的操作的重要性.



[^1]: 这个描述貌似和上面对 working tree 的描述有冲突, 上面对 working tree 的说法是 working tree 包含了 `.git` 子目录,但是出于译者对 commit 的认识而言, commit 中应该没有 `.git` 中的相关信息. 这里可以尝试理解为文中说的 working tree 是类似工作区的概念, 并不包括 `.git` 子目录

[^2]: 这个地方括号中的内容不知道怎么翻译, 可以理解为, "我们还有很多关于 commit 的东西马上就要谈到", 但是感觉和上下文不搭.

[^3]: 存疑





## Repository: Directory content tracking | Repository: 目录内容跟踪



> As mentioned above, what Git does is quite rudimentary: it maintains snapshots of a directory’s contents. Much of its internal design can be understood in terms of this basic task.

就像上面提到的一样, Git 实际上做的事情非常的基础: 只是在维护一个目录里的内容的快照. Git 的许多内部设计都可以从这个角度来理解.



> The design of a Git repository in many ways mirrors the structure of a UNIX filesystem:

Git 的 repository 的设计很多时候都和 UNIX 的文件系统很相似:

>  A _filesystem_ begins with a root directory, which typically consists of other directories, most of which have leaf nodes, or _files_, that contain data. Meta-data about these files’ contents is stored both in the directory (the names), and in the i-nodes that reference the contents of those files (their size, type, permissions, etc). Each _i-node_ has a unique number that identifies the contents of its related file. And while you may have many directory entries pointing to a particular i-node (i.e., hard-links), it’s the i-node which “owns” the contents stored on your filesystem.

文件系统是从根目录开始的,  通常它会包括其他的所有目录. 大部分的目录会含有叶子节点或者说是文件, 文件中才真正含有数据. 关于文件内容的元数据同时被存放在目录里, 以及引用这个文件的内容的那个 i-node 里. 在目录里只存了个文件名, 而在 i-node 中存放着文件的大小, 类型, 权限等等. 每个 i-node 有一个唯一的编码, 用于区分它关联的文件内容, 但是要注意: 是可能会出现多个文件目录指向了同一个 i-node 的情况的(比如说硬链接). 总的来说,是 i-node 最终管理着你存在文件系统里的数据文件. 



> Internally, Git shares a strikingly similar structure, albeit with one or two key differences.

从内部逻辑的角度来看, Git 除了一两处关键的不同之处以外, 与这个结构有着惊人的相似. 

>  First, it represents your file’s contents in _blobs_, which are also leaf nodes in something awfully close to a directory, called a _tree_. Just as an i-node is uniquely identified by a system-assigned number, a blob is named by computing the SHA1 hash id of its size and contents. 

首先, Git 将你的文件内容以 _blob_ 的方式呈现. 而 _blob_ 也是一个和目录十分类似的树形结构中的叶子节点, 这里我们直接将这种树形结构称为 _tree_. 和 i-node 有一个系统赋予的, 独一无二的值一样, 一个 blob 也有一个独一无二的标识符, 而这个标识符也就是这个 blob 的名字是从它的大小和内容运用 SHA1 哈希算法计算得出的.

> For all intents and purposes this is just an arbitrary number, like an i-node, except that it has two additional properties: 

 这个值具体是什么并不重要, 在 Git 中我们基本上就是需要一个值来唯一地确定一个 blob, 这一点上和 i-node 很类似. 但是这个通过 SHA1 计算出来的哈希值却有两个额外的性质:

> first, it verifies the blob’s contents will never change; and second, the same contents shall always be represented by the same blob, no matter where it appears: across commits, across repositories — even across the whole Internet. If multiple trees reference the same blob, this is just like hard-linking: the blob will not disappear from your repository as long as there is at least one link remaining to it.

首先, 这个值能够保证一个 blob 的内容不会被随意更改; 其次, 只要 blob 的内容一样, 他们总会有同一个标识符, 即使他们在不同的地方也一样: 在不同的 commit 之间相同内容的 blob 会有同一个名字, 不同仓库间的 blob 也一样, 即使是网络上的其他地方的 blob 也一样. 如果有多个 tree 引用了同一个 blob, 这情况几乎就和硬链接没什么两样: 直到没有 tree 引用这个 blob 了, 它才会从你的 repository 中消失.



> The difference between a Git blob and a filesystem’s file is that a blob stores no metadata about its content. All such information is kept in the tree that holds the blob. One tree may know those contents as a file named “foo” that was created in August 2004, while another tree may know the same contents as a file named “bar” that was created five years later. 

Git 中的 blob 和文件系统中的文件的一大区别就是 blob 中并没有存储与其内容相关的元数据. 这些信息是存放在引用这个 blob 的 tree 中的. 某个 tree 也许会觉得这个 blob 中的内容是一个叫 "foo" 的, 于 2004 年八月创建的文件, 同时另一个 tree 却会认为同样的一个 blob 对应着名叫 "bar" 的, 五年后才被创建的文件.

> In a normal filesystem, two files with the same contents but with such different metadata would always be represented as two independent files. Why this difference? Mainly, it’s because a filesystem is designed to support files that change, whereas Git is not. The fact that data is immutable in the Git repository is what makes all of this work and so a different design was needed. And as it turns out, this design allows for much more compact storage, since all objects having identical content can be shared, no matter where they are.

通常来说, 在文件系统中, 两个内容一样却有不一样的元数据的文件总是会被当作是两个独立的文件. 为什么会有这种不同呢? 基本上我们可以认为这是因为文件系统主要是为了会变化的文件设计的, 而 Git 并不是, 我们反而是希望在 Git 中被存下来的快照不会发生变化的. 实际上这种设计甚至可以用来压缩存储空间, 因为它允许两个有相同内容的文件共享空间[^4].



[^4]: 有理由相信 github 可以用这个方式来省钱, 这样 fork 出来的 repository 就不会那么占空间了.





## Introducing the blob | 简单介绍一下 blob



> Now that the basic picture has been painted, let’s get into some practical examples. I’m going to start by creating a sample Git repository, and showing how Git works from the bottom up in that repository. Feel free to follow along as you read: 

我们已经大致了解了 Git 的工作原理, 现在让我们看一些实践中的案例. 我将创建一个 Git repository 作为例子, 用它来自底向上地展示 Git 是如何工作的. 你完全可以在阅读的同时也自己动手试试:



```bash
$ mkdir sample; cd sample
$ echo 'Hello, world!' > greeting
```



> Here I’ve created a new filesystem directory named “sample” which contains a file whose contents are prosaically predictable. I haven’t even created a repository yet, but already I can start using some of Git’s commands to understand what it’s going to do. First of all, I’d like to know which hash id Git is going to store my greeting text under:

这里我创建了一个叫叫 "sample" 的文件夹, 它的内容是显而易见的. 目前我还没有创建一个 repository, 但是我实际上已经可以使用一些 Git 命令来简单解释一下接下来我们要干些啥. 首先, 我想知道一下这个叫 greeting 的文件被使用 Git 进行版本管理之后, 它对应的那个 blob 的名字是啥:



```bash
$ git hash-object greeting
af5626b4a114abcb82d63db7c8082c3c4756e51b
```



> If you run this command on your system, you’ll get the same hash id. Even though we’re creating two different repositories (possibly a world apart, even) our greeting blob in those two repositories will have the same hash id. I could even pull commits from your repository into mine, and Git would realize that we’re tracking the same content — and so would only store one copy of it! Pretty cool.

如果你也在你的机子上跑了这条命令,你一定会和我得到一个同样的哈希值. 虽然我们甚至可能不在同一个位面 :smirk:[^5], 在两个不同的 repository 中, 同样的内容还是有同样的哈希值. 我甚至可以从你的 repository 中 pull 一些 commit 到我这里来, 如果我真的那么做了, Git 也会认识到我们两个人是在跟踪同样的一个内容 — 所以它也只会存储一份数据. 这真的很棒!



> The next step is to initialize a new repository and commit the file into it. I’m going to do this all in one step right now, but then come back and do it again in stages so you can see what’s going on underneath:

接下来我们创建一个新的 repository 然后把文件 commit 进去. 我接下来将一次性完成它, 之后我会再一步步的做一次, 这样有利于你理解到底发生了什么: 



```bash
$ git init
$ git add greeting
$ git commit -m "Added my greeting"
```



> At this point our blob should be in the system exactly as we expected, using the hash id determined above. As a convenience, Git requires only as many digits of the hash id as are necessary to uniquely identify it within the repository. Usually just six or seven digits is enough:

此时, 我们的那个 blob 应该正如我们预想的那样出现在这个系统中了, 它的哈希值正如上面所说的那样, 应该是`af5626b4a114abcb82d63db7c8082c3c4756e51b`. 方便起见, 只要你给出足以在这个 repository 中唯一确定这个 blob 的哈希值前缀, Git 就可以找到这样一个 blob. 通常来说六七个字符就够了:



```bash
$ git cat-file -t af5626b
blob
$ git cat-file blob af5626b
Hello, world!
```



> There it is! I haven’t even looked at which commit holds it, or what tree it’s in, but based solely on the contents I was able to assume it’s there, and there it is. It will always have this same identifier, no matter how long the repository lives or where the file within it is stored. These particular contents are now verifiably preserved, forever.

看, 我们已经找到他了, 我甚至都不需要知道是哪个 commit 引用了这个 blob, 也不用知道这个 blob 在哪个 tree 上, 我们只需要通过文件的内容就足以确定这个 blob 的存在. 这个文件一直都可以以这种方式来唯一确定, 不管这个repository存在了多久, 也不用管这个 blob 到底被存储在 repository 中的哪里. 这个内容已经被很好的保护了起来.

> In this way, a Git blob represents the fundamental data unit in Git. Really, the whole system is about blob management.

Git 以这种方式在 repository 中呈现数据. 讲真, 整个 Git 都只是在管理 blob 而已.



[^5]:  这个地方我皮了一下, 但是实际上我并不确定原文是不是这个意思





# Blobs are stored in trees | Blob 是存在 tree 里的



> The contents of your files are stored in blobs, but those blobs are pretty featureless. They have no name, no structure — they’re just “blobs”, after all.

Git 在追踪文件的过程中, 将你文件里的内容存储在 blob 中, 但是 blob 的功能相当的有限, 它们没有文件名, 没有结构, 它们只是 "blob", 仅此而已.



> In order for Git to represent the structure and naming of your files, it attaches blobs as leaf nodes within a tree. Now, I can’t discover which tree(s) a blob lives in just by looking at it, since it may have many, many owners. But I know it must live somewhere within the tree held by the commit I just made:

为了能够记录下你文件的名字和结构, Git 将 blob 看作是 tree 的叶子节点来管理. 我们显然不能通过查看一个 blob 来看出它到底属于哪个 tree, 因为它很可能同时出现在相当多个 tree 中. 但是我知道它一定存在于哪个我们刚刚创建的 commit 所管理的 tree 中: 



```bash
$ git ls-tree HEAD
100644 blob af5626b4a114abcb82d63db7c8082c3c4756e51b greeting
```



> There it is! This first commit added my greeting file to the repository. This commit contains one Git tree, which has a single leaf: the greeting content’s blob.

这就是那个 blob 了! 这是我刚提交的那个, 将我的 greeting 文件添加到 repository 中的 commit. 这个 commit 里面有一个 Git tree, 而这个 tree 中只有一个叶子节点 — 那就是存放着 greeting 文件内容的那个 blob. 

> Although I can look at the tree containing my blob by passing HEAD to `ls-tree`, I haven’t yet seen the underlying tree object referenced by that commit. Here are a few other commands to highlight that difference and thus discover my tree:

尽管我可以通过向 `ls-tree` 这个命令传入 HEAD 来查看那个含有我的 blob 的 tree, 但是我还没从底层看到过那个被 commit 引用的 tree 对象. 这里有几个命令能说明这两者有什么不同, 我们来看看:[^6]



```bash
$ git rev-parse HEAD
588483b99a46342501d99e3f10630cfc1219ea32 # different on your system

$ git cat-file -t HEAD
commit

$ git cat-file commit HEAD
tree 0563f77d884e4f79ce95117e2d686d7d6e282887
author John Wiegley <johnw@newartisans.com> 1209512110 -0400
committer John Wiegley <johnw@newartisans.com> 1209512110 -0400
Added my greeting
```



> The first command decodes the HEAD alias into the commit it references, the second verifies its type, while the third command shows the hash id of the tree held by that commit, as well as the other information stored in the commit object. The hash id for the commit is unique to my repository — because it includes my name and the date when I made the commit — but the hash id for the tree should be common between your example and mine, containing as it does the same blob under the same name.

第一个命令通过 HEAD 找到它所指向的那个 commit, 第二个可以指明 HEAD 指针的类型, 而第三个命令则展示了那个 commit 所管理的 tree 的哈希值, 以及这个 commit 的一些相关信息. 这个 commit 的哈希值和你的系统中得到的是不同的, 因为如你所见, 一个 commit 中式包含作者和提交时间,还有说明文字等信息的, 这些东西都会加入哈希值的计算, 所以一定是不同的. 然而 tree 的哈希值却和你的系统中是一样的. 因为我们两的系统中, 这个 tree 的内容是完全一致的.

> Let’s verify that this is indeed the same tree object:

我们来确认一下这个 tree 是真的和你那边相同:



```bash
$ git ls-tree 0563f77
100644 blob af5626b4a114abcb82d63db7c8082c3c4756e51b greeting
```



> There you have it: my repository contains a single commit, which references a tree that holds a blob — the blob containing the contents I want to record. There’s one more command I can run to verify that this is indeed the case:

这里你可以看到, 我的 repository 中只包含一个 commit, 这个 commit 它关联到一个 tree, 而这个 tree 管理了一个 blob, 这个 blob 里面是我希望记录下来的内容. 还有一个命令也能展示这个情况:



```bash
$ find .git/objects -type f | sort
.git/objects/05/63f77d884e4f79ce95117e2d686d7d6e282887
.git/objects/58/8483b99a46342501d99e3f10630cfc1219ea32
.git/objects/af/5626b4a114abcb82d63db7c8082c3c4756e51b
```

 

> From this output I see that the whole of my repo contains three objects, each of whose hash id has appeared in the preceding examples. Let’s take one last look at the types of these objects, just to satisfy curiosity:

在这个命令的输出中可以看到我的整个 repository 中只有三个对象, 每一个的哈希值你都已经见到过了. 为了满足好奇心, 我们甚至可以看一下他们对应的类型:



```bash
$ git cat-file -t 588483b99a46342501d99e3f10630cfc1219ea32
commit
$ git cat-file -t 0563f77d884e4f79ce95117e2d686d7d6e282887
tree
$ git cat-file -t af5626b4a114abcb82d63db7c8082c3c4756e51b
blob
```



> I could have used the show command at this point to view the concise contents of each of these objects, but I’ll leave that as an exercise to the reader.

我本可以使用 show 命令来查看这些对象的内容, 但是我选择将这个留作读者的练习.



[^6]:存疑





## How trees are made | tree 是怎样炼成的



> Every commit holds a single tree, but how are trees made? We know that blobs are created by stuffing the contents of your files into blobs — and that trees own blobs — but we haven’t yet seen how the tree that holds the blob is made, or how that tree gets linked to its parent commit.

每个 commit 对应一个 tree, 但是 tree 是怎样炼成的呢? 我们已经知道了 blob 是基于你的文件内容创建的数据块, 以及 Git 是用 tree 来管理 blob 的. 但是我们还并不理解这些管理 blob 用的 tree 是如何被创建的, 或者说我们并不懂 tree 是如何连接到它的父 commit 的.

> Let’s start with a new sample repository again, but this time by doing things manually, so you can get a feeling for exactly what’s happening under the hood:

我们来看上一节中提到的那个例子, 不过这次我们一步一步手动地做这些事情, 这样你可以明白底层到底发生了什么:

```bash
$ rm -fr greeting .git
$ echo 'Hello, world!' > greeting
$ git init
$ git add greeting
```

> It all starts when you first add a file to the index. For now, let’s just say that the index is what you use to initially create blobs out of files. When I added the file `greeting`, a change occurred in my repository. I can’t see this change as a commit yet, but here is one way I can tell what happened:

这一切是从你首次将一个文件加入 the index 开始的. 现在让我们先这么认为: the index 是你初次用来将文件内容装填进 blob 时所需要使用的工具.当我添加名为 `greeting` 的文件时, 我的 repository 发生了一次修改, 而我目前还不能够将这个更改看作是 commit, 但是我还是有一个方法能够搞清到底发生了什么:

```bash
$ git log # this will fail, there are no commits!
fatal: bad default revision 'HEAD'
$ git ls-files --stage # list blob referenced by the index
100644 af5626b4a114abcb82d63db7c8082c3c4756e51b 0 greeting
```

> What’s this? I haven’t committed anything to the repository yet, but already an object has come into being. It has the same hash id I started this whole business with, so I know it represents the contents of my `greeting` file. I could use `cat-file -t` at this point on the hash id, and I’d see that it was a blob. It is, in fact, the same blob I got the first time I created this sample repository. The same file will always result in the same blob (just in case I haven’t stressed that enough).

那么这是啥呢? 我还没有 commit 过任何东西啊? 但是实际上已经有一个东西在上述的操作中被生成了出来. 它的哈希值和我们一开始对 `greeting` 算出来的那个是一样的, 所以我们可以知道这个东西它里面是文件 `greeting` 的数据. 此时, 我可以使用命令 `cat-file-t` 来查看那个哈希值对应的文件, 实际上我们也看过了, 这个文件本身是一个 blob. 实际上它和我们一开始创建的那个 repository 中的那个 blob 是完全一样的东西. 

> This blob isn’t referenced by a tree yet, nor are there any commits. At the moment it is only referenced from a file named `.git/index`, which references the blobs and trees that make up the current index. So now let’s make a tree in the repo for our blob to hang off of:

这个 blob 目前还没有被某个 tree 引用, 实际上也没有被任何一个 commit 引用. 在这个时候, 实际上只有一个叫 `.git/index`的文件引用了这个 blob, 这文件里面引用了所有构成了当前的 index 的 blob 和 tree. 那么我们现在用这个 blob 来创建一个 tree:

```bash
$ git write-tree # record the contents of the index in a tree
0563f77d884e4f79ce95117e2d686d7d6e282887
```

> This number should look familiar as well: a tree containing the same blobs (and sub-trees) will always have the same hash id. I don’t have a commit object yet, but now there is a tree object in that repository which holds the blob. The purpose of the low-level `write-tree` command is to take whatever the contents of the index are and tuck them into a new tree for the purpose of creating a commit.

这个值也令人十分熟悉: 两个包含同样的 blob, 或者说 sub-tree 们的 tree 的哈希值总时一样的. 我的 repository 中到现在还没有一个 commit, 但是已经有一个 tree 了, 这个 tree 还引用了一个 blob. `wirte-tree` 这个底层命令的存在, 使得我们可以将任何在 the index 中的内容装载到一个新的 tree 中, 然后用来创建一个 commit.

> I can manually make a new commit object by using this tree directly, which is just what the `commit-tree` command does:

只需要用 `commit-tree` 命令, 我现在就可以用上面创建的 tree 来手动地创建一个新的 commit 对象, 就像这样:

```bash
$ echo "Initial commit" | git commit-tree 0563f77
5f1bc85745dcccce6121494fdd37658cb4ad441f
```

> The raw `commit-tree` command takes a tree’s hash id and makes a commit object to hold it. If I had wanted the commit to have a parent, I would have had to specify the parent commit’s hash id explicitly using the `-p` option. Also, note here that the hash id differs from what will appear on your system: This is because my commit object refers to both my name, and the date at which I created the commit, and these two details will always be different from yours.

这个 `commit-tree` 命令需要知道那个 tree 的哈希值, 然后创建一个 commit 来管理它. 如果我想要这个 commit 有一个父 commit, 那我需要使用 `-p` 参数指定父 commit 的哈希值. 以及我们可以注意到, 这个 tree 的哈希值和你的系统上的是不同的, 这是因为我的 commit 中实际上包含了我的名字信息, 以及我创建这个 commit 的日期, 这两个值是和你的仓库中的情况不同的.

> Our work is not done yet, though, since I haven’t registered the commit as the new head of a branch:

活还没完, 我们还没更新这个 commit 对应的分支:

```bash
$ echo 5f1bc85745dcccce6121494fdd37658cb4ad441f > .git/refs/heads/master
```

> This command tells Git that the branch name “master” should now refer to our recent commit. Another, much safer way to do this is by using the command `update-ref`:

这个命令告诉 Git 该用 "master" 这个名字指代我们新提交的那个 commit 了. 还有一点要说的就是, 完成上面这件事情还有一个更安全的方法, 那就是使用 `update-ref` 命令:

```bash
$ git update-ref refs/heads/master 5f1bc857
```

> After creating `master`, we must associate our working tree with it. Normally this happens for you whenever you check out a branch:

在创建[^7]完 master 之后,我们必须将我们的 working tree 调整成和 master 相匹配的样子. 一般来说, 这个操作是用于 check out 一个分支的:

```bash
$ git symbolic-ref HEAD refs/heads/master
```

> This command associates HEAD symbolically with the master branch. This is significant because any future commits from the working tree will now automatically update the value of `refs/heads/master`.

这个命令会将 HEAD 绑定到 master 分支上. 这一点是相当自然的, 因为我们接下来再从 working tree 中做出更改之后提交的 commit 应该要更新这个名叫 master 的分支, 也就是文件 `refs/heads/master` 的值.



> It’s hard to believe it’s this simple, but yes, I can now use log to view my newly minted commit:

虽然很难相信, 但是 Git 的工作就是如此简单, 我还可以使用 log 命令来查看我刚一步步手动添加的这个 commit:

```bash
$ git log
commit 5f1bc85745dcccce6121494fdd37658cb4ad441f
Author: John Wiegley <johnw@newartisans.com>
Date:   Mon Apr 14 11:14:58 2008 -0400
        Initial commit
```

> A side note: if I hadn’t set `refs/heads/master` to point to the new commit, it would have been considered “unreachable”, since nothing currently refers to it nor is it the parent of a reachable commit. When this is the case, the commit object will at some point be removed from the repository, along with its tree and all its blobs. (This happens automatically by a command called `gc`, which you rarely need to use manually). By linking the commit to a name within `refs/heads`, as we did above, it becomes a reachable commit, which ensures that it’s kept around from now on.

这里有个值得注意的地方: 如果我不曾将文件 `refs/heads/master` 指向那个新的 commit, 这个 commit 会被看作是 "不可达的", 因为没有任何其他的东西和他有关联, 它也不是任何一个可达的 commit 的祖先. 当发生这种情况的时候, 这个 commit 将会被 Git 在某个时候从 repository 中移除, 它对应的 tree 和 blob 也一样, (这个事情会在运行一个叫 `gc` 的命令的时候自动地发生, 当然这个命令基本不会被用户手动运行). 如果我们像上面做的那样, 在目录 `.git/heads` 下用某个名字关联到这个 commit, 这个 commit 就是可达的了, 这可以保证它不会被删掉.



[^7]: 这个时候真的还没有 "master" 这个分支么? 为什么可以说是 create 呢?



