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

[ 译者注: 这个描述貌似和上面对 working tree 的描述有冲突, 上面对 working tree 的说法是 working tree 包含了 `.git` 子目录,但是出于译者对 commit 的认识而言, commit 中应该没有 `.git` 中的相关信息. 这里可以尝试理解为文中说的 working tree 是类似工作区的概念, 并不包括 `.git` 子目录 ]



### branch



> * **branch** — A **branch** is just a name for a commit (and much more will be said about commits in a moment), also called a reference. It’s the parentage of a commit which defines its history, and thus the typical notion of a “branch of development”.

* **branch** — **branch** 只是某个的 commit 的别名, 也可以认为是一个引用. 它是一个 commit 的来历, 记录着我们是如何到达这个 commit 的. 这很好地体现了分支开发的理念.

[ 译者注: 这个地方括号中的内容不知道怎么翻译, 可以理解为, "我们还有很多关于 commit 的东西马上就要谈到", 但是感觉和上下文不搭. ]



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
  * 如果你 checkout 一个 branch, HEAD 就会指向那个 branch, 这意味着这个 branch 在下一次 commit 之后会更新.  
  
  * 如果你 checkout 某个特定的 commit, 那么 HEAD 只会指向那个 commit. 发生这种事情的时候, 我们称一个 HEAD 处在脱离的状态. 当你 checkout 一个 tag 的时候也会发生这样的事情.



> The usual flow of events is this: After creating a repository, your work is done in the working tree. Once your work reaches a significant point — the completion of a bug, the end of the working day, a moment when everything compiles — you add your changes successively to the index. Once the index contains everything you intend to commit, you record its content in the repository. Here’s a simple diagram that shows a typical project’s life-cycle:

通常来说, 你用 Git 工作的流程是这样的: 在创建一个 repository 之后, 你在 working tree 中完成工作. 每当你完成了一个阶段的工作, 比方说修正了一个 bug, 或者下班了, 还有比如"终于能过编译了!",你就把所有的更改都一个一个添加到 the index 中. 当 the index 中包含了所有你想 commit 的内容, 你就将这个 commit 的内容加入到 repository 中.这里有一个简单的示意图,展示了一个项目通常的生命周期:



![Project Lifecycle](https://jwiegley.github.io/git-from-the-bottom-up/images/lifecycle.png)



> With this basic picture in mind, the following sections shall attempt to describe how each of these different entities is important to the operation of Git.

请将上述的内容记在心里, 接下来的几节我们将说明这些东西对 Git 的操作的重要性.





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

通常来说, 在文件系统中, 两个内容一样却有不一样的元数据的文件总是会被当作是两个独立的文件. 为什么会有这种不同呢? 基本上我们可以认为这是因为文件系统主要是为了会变化的文件设计的, 而 Git 并不是, 我们反而是希望在 Git 中被存下来的快照不会发生变化的. 实际上这种设计甚至可以用来压缩存储空间, 因为它允许两个有相同内容的文件共享空间.

[ 译者注: 有理由相信 github 可以用这个方式来省钱, 这样 fork 出来的 repository 就不会那么占空间了. ]





## Introducing the blob | 关于 blob 的二三事



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

如果你也在你的机子上跑了这条命令,你一定会和我得到一个同样的哈希值. 虽然我们甚至可能不在同一个位面 :smirk:, 在两个不同的 repository 中, 同样的内容还是有同样的哈希值. 我甚至可以从你的 repository 中 pull 一些 commit 到我这里来, 如果我真的那么做了, Git 也会认识到我们两个人是在跟踪同样的一个内容 — 所以它也只会存储一份数据. 这真的很棒!

[ 译者注: 这个地方我皮了一下, 但是实际上我并不确定原文是不是这个意思 ]



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

尽管我可以通过向 `ls-tree` 这个命令传入 HEAD 来查看那个含有我的 blob 的 tree, 但是我还没从底层看到过那个被 commit 引用的 tree 对象. 这里有几个命令能说明这两者有什么不同, 我们来看看:

[ 译者注: 最后这句话的翻译不确定是否正确 ]



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





## How trees are made | Tree 是怎样炼成的



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

在创建完 master 之后,我们必须将我们的 working tree 调整成和 master 相匹配的样子. 一般来说, 这个操作是用于 check out 一个分支的:

[译者注: 这个时候真的还没有 "master" 这个分支么? 为什么可以说是 create 呢? ]



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





# The beauty of commits | Commit 之美



>  Some version control systems make “branchCes” into magical things, often distinguishing them from the “main line” or “trunk”, while others discuss the concept as though it were very different from commits. But in Git there are no branches as separate entities: there are only blobs, trees and commits. Since a commit can have one or more parents, and those commits can have parents, this is what allows a single commit to be treated like a branch: because it knows the whole history that led up to it.

一些版本控制系统将 branch 弄得很晦涩难懂, 经常将 "主线" (或者说 "主干" ), 和 "分支" 区分开来, 而另一些版本控制系统则认为分支这个概念和 commit 有很大区别. 但是在 Git 中并没有 branch 这个实体: Git 里只有 blob, tree 以及 commit 这三个概念. 因为一个 commit 可以有多个父 commit 而这些父 commit 也有父亲, 所以我们实际上可以把单个 commit 当作 branch 来看待: 因为我们实际上可以从这个 commit 开始, 回溯出文件内容是如何一步步地被修改, 更迭到当前这个 commit 的这整个历史.

> You can examine all the top-level, referenced commits at any time using the `branch` command:

你随时都可以通过 `branch` 命令查看所有顶层的, 被引用的 commit :



```bash
$ git branch -v
* master 5f1bc85 Initial commit
```



> Say it with me: A branch is nothing more than a named reference to a commit. In this way, branches and tags are identical, with the sole exception that tags can have their own descriptions, just like the commits they reference. Branches are just names, but tags are descriptive, well, “tags”.

来跟着我一起念: "一个 branch 仅仅只是一个 commit 的引用, 只是这个引用本身还有一个名字罢了." 从这种角度来看, branch 和 tags 几乎没有什么区别,  只有一个例外: tag 就像它指向的那个 commit 一样还可以拥有自己的说明性文字. branch 只是 commit 的名字, 但是 tag 是描述性的, 毕竟人家是 "标签" 嘛.

> But the fact is, we don’t really need to use aliases at all. For example, if I wanted to, I could reference everything in the repository using only the hash ids of its commits. Here’s me being straight up loco and resetting the head of my working tree to a particular commit:

但是实际上我们也可以完全就不用这些别名. 比如说, 如果我想的话, 我实际上可以用 repository 中任何一个 commit 的哈希值来唯一地确定它. 下面这个是如果我想直接把我的 working tree 对应的 HEAD 指向某个特定的 commit 需要用的命令:



```bash
$ git reset --hard 5f1bc85
```



> The --hard option says to erase all changes currently in my working tree, whether they’ve been registered for a checkin or not (more will be said about this command later). A safer way to do the same thing is by using `checkout`:

这个 `--hard` 选项的意思就是说, 让 Git 不管我当前的 working tree 中相对于给定的 commit 发生的所有更改是否有被记录下来过, 都把它们清除掉. 我们以后还会聊聊 `reset` 这个命令. 完成这件事情还有一个更安全的方式, 那就是使用 checkout 命令:



```bash
$ git checkout 5f1bc85
```



> The difference here is that changed files in my working tree are preserved. If I pass the `-f` option to `checkout`, it acts the same in this case to `reset --hard`, except that checkout only ever changes the working tree, whereas `reset --hard` changes the current branch's HEAD to reference the specified version of the tree.

这两者的区别在于, 后者对于我的 working tree 中的文件修改操作是有一定保护的, 对于没记录过的数据会做一次是否清除的询问.  如果我们将参数 `-f` 传递给 `checkout`命令, Git 的行为就会几乎和执行 `reset --hard` 时一模一样. 它们两之间的区别在于 `checkout` 命令只是清除 working tree 中的文件变化, 而 `reset --hard` 会将当前的 HEAD 指向的那个 branch 一起移动到某个版本的 tree 上.



> Another joy of the commit-based system is that you can rephrase even the most complicated version control terminology using a single vocabulary. For example, if a commit has multiple parents, it’s a “merge commit” — since it merged multiple commits into one. Or, if a commit has multiple children, it represents the ancestor of a “branch”, etc. But really there is no difference between these things to Git: to it, the world is simply a collection of commit objects, each of which holds a tree that references other trees and blobs, which store your data. Anything more complicated than this is simply a device of nomenclature.

另一个使用基于 commit 的版本控制系统带来的乐趣是你可以用很简单的话语来描述版本控制系统中那些晦涩难懂的术语. 比方说, 如果一个 commit 拥有两个父 commit, 那么我们称这个 commit 是一个 "merge commit" — 因为它确实将多个 commit 合并了起来嘛.还有,如果一个 commit 拥有多个子 commit 这代表着这里是分支的开始, 诸如此类. 但是实际上这些术语所描述的事情对于 Git 来说并没有什么区别: 对于 Git 来说, 整个世界就是由 commit 对象组成的, 它们管理着一个 tree 而这个 tree 中又引用了 其他的 tree 以及 blob, 在 blob 中实际存放着你的数据. 其他比这个复杂的东西都只是其他人对某个行为取的名字罢了.

> Here is a picture of how all these pieces fit together:

这里有一张关于上面提到的种种对象之间的关系的图,也许可以帮助你的理解:



![Commits](https://jwiegley.github.io/git-from-the-bottom-up/images/commits.png)





# A commit by any other name... | Commit 的名字



> Understanding commits is the key to grokking Git. You’ll know you have reached the Zen plateau of branching wisdom when your mind contains only commit topologies, leaving behind the confusion of branches, tags, local and remote repositories, etc. Hopefully such understanding will not require lopping off your arm — although I can appreciate if you’ve considered it by now.

对 commit 的理解对于弄懂 Git 来说非常关键. 当你抛弃了那些令人困惑的什么 branch, tag, 本地的以及远程 repository 之后, 当你的头脑中只剩下了 commit 相关的拓扑结构的时候, 你将会对 "分支" 的智慧有崭新的理解. 想过去这种对 commit 的理解对于你来不会是什么很困难的事情, 如果你现在就能开始试图理解 commit 我会很开心.



> If commits are the key, how you name commits is the doorway to mastery. There are many, many ways to name commits, ranges of commits, and even some of the objects held by commits, which are accepted by most of the Git commands. Here’s a summary of some of the more basic usages:

如果说 commit 是关键, 那么如何给 commit 命名就是理解 Git 的门道所在. 实际上有很多很多给 commit 命名的方式, 甚至连一次性给某个范围内的 commit 命名, 乃至于给一些由 commit 管理的对象命名, 都是可以办到的. 大部分的 Git 命令都支持这种操作. 这里有一些你需要知道的用法总结:



> * **branchname** — As has been said before, the name of any branch is simply an alias for the most recent commit on that “branch”. This is the same as using the word HEAD whenever that branch is checked out.

* **branchname** — 就像之前说的那样, branch 的名字只是那个 "分支" 上最新的一个 commit 的别名. 这和你使用单词 HEAD 来表示当前 check out 的那个 branch 是一样的. 



> * **tagname** — A tag-name alias is identical to a branch alias in terms of naming a commit. The major difference between the two is that tag aliases never change, whereas branch aliases change each time a new commit is checked in to that branch.

* **tagname** — 从给commit命名的角度来看, 一个 tag 几乎和一个 branch 是一模一样的. 主要的区别在于 branch 指代的 commit 是可能会变化的, 它会在新的 commit 提交到这个 branch 的时候被更新成新的那个, 而 tag 不会.



>* **HEAD** — The currently checked out commit is always called HEAD. If you check out a specific commit — instead of a branch name — then HEAD refers to that commit only and not to any branch. Note that this case is somewhat special, and is called “using a detached HEAD” (I’m sure there’s a joke to be told here...).

* **HEAD** — 当前 check out 的那个 commit 被称作 HEAD. 如果你 check out 了一个特定的 commit — 而不是一个 branch 的话 — 那么 HEAD 就只是指向了那个 commit 而已, 并没有指向任何一个 branch. 需要注意的是这是一种特殊的情况, 我们称这种情况为 HEAD 指针的脱离.



> * **c82a22c39cbc32...** — A commit may always be referenced using its full, 40-character SHA1 hash id. Usually this happens during cut-and-pasting, since there are typically other, more convenient ways to refer to the same commit.
> * **c82a22c** — You only need use as many digits of a hash id as are needed for a unique reference within the repository. Most of the time, six or seven digits is enough.

* **c82a22c39cbc32...** — 我们总是可以通过一个 commit 的那个由四十个字符组成的 SHA1 哈希值来找到它. 通常这种事情发生在你需要复制粘贴的时候, 因为在其他情况下, 几乎都有更方便的方式来找到它.
* **c82a22c** — 其实你只需要给出足够在当前的 repository 中唯一确定一个 commit 的哈希值的前缀就足以找到那个 commit 了, 一般来说是 6 到 7 位左右.



> * **name^** — The parent of any commit is referenced using the caret symbol. If a commit has more than one parent, the first is used.
> * **name^^** — Carets may be applied successively. This alias refers to “the parent of the parent” of the given commit name.
> * **name^2** — If a commit has multiple parents (such as a merge commit), you can refer to the _nth_ parent using `name^n`.
> * **name~10** — A commit’s _nth_ generation ancestor may be referenced using a tilde (~) followed by the ordinal number. This type of usage is common with `rebase -i`, for example, to mean “show me a bunch of recent commits”. This is the same as name^^^^^^^^^^.
> * **name:path** — To reference a certain file within a commit’s content tree, specify that file’s name after a colon. This is helpful with show, or to show the difference between two versions of a committed file:

接下来的几条中的 "name" 可以使用上文提到的所有可以找到 commit 的字符串替换, 以下是一些相对某个 commit 查询另一个 commit 的方法:

* **name^** —  `^` 这个符号可以找到一个 commit 的父 commit,  如果这个 commit 有多个父 commit 那么查询的结果是第一个.

[ 译者注: 这里的 "first" 是按提交 commit 的时间排序的么? ]

* **name^^** — `^` 这个符号是可以被连续的调用的 , 这意味着你寻找的是当前 commit 的爷爷.

[ 译者注: 毕竟你可以将 "name^" 看作是一个 "name" 嘛 ]

* **name^2** — 如果一个 commit 有多个父 commit — 比方说一个 merge commit — 那么如果你想找其中的第 n 个父 commit, 可以使用 `name^n`.
* **name~10** — 这意味着一个 commit 十代之前的那个祖宗, 第 n 代祖先,可以通过一个 `~` 符号后面跟着一个数字来找到. 这一般是用于执行 `rebase -i` 命令的时候用的. 效果和 name^^^^^^^^^^ 是完全一致的.
* **name:path** — 你可以用这种方式来从一个 commit 中根据路径来找到 tree 中的某个特定文件. 一般来说这个功能在你想比较两个 commit 之间某个文件的差异的时候比较有用, 比如像下面这样:



```bash
  $ git diff HEAD^1:Makefile HEAD^2:Makefile
```



> * **name^{tree}** — You can reference just the tree held by a commit, rather than the commit itself.

* **name^{tree}** — 上面说的都是找到某个 commit 的方法, 而你可以通过这个方式来指定一个 commit 管理的那个 tree.



>* **name1..name2** — This and the following aliases indicate _commit ranges_, which are supremely useful with commands like log for seeing what’s happened during a particular span of time. The syntax to the left refers to all the commits reachable from **name2** back to, but not including, **name1**. If either **name1** or **name2** is omitted, HEAD is used in its place.
>* **name1...name2** — A “triple-dot” range is quite different from the two-dot version above. For commands like log, it refers to all the commits referenced by **name1** or **name2**, but not by both. The result is then a list of all the unique commits in both branches. For commands like `diff`, the range expressed is between **name2** and the common ancestor of **name1** and **name2**. This differs from the `log` case in that changes introduced by **name1** are not shown.
>* **master..** — This usage is equivalent to “`master..HEAD`”. I’m adding it here, even though it’s been implied above, because I use this kind of alias constantly when reviewing changes made to the current branch.
>* **..master** — This, too, is especially useful after you’ve done a `fetch` and you want to see what changes have occurred since your last `rebase` or `merge`.
>* **--since="2 weeks ago"** — Refers to all commits since a certain date.
>* **--until=”1 week ago”** — Refers to all commits before a certain date.
>* **--grep=pattern** — Refers to all commits whose commit message matches the regular expression pattern.
>* **--committer=pattern** — Refers to all commits whose committer matches the pattern.
>* **--author=pattern** — Refers to all commits whose author matches the pattern. The author of a commit is the one who created the changes it represents. For local development this is always the same as the committer, but when patches are being sent by e-mail, the author and the committer usually differ.
>* **--no-merges** — Refers to all commits in the range that have only one parent — that is, it ignores all merge commits.

以下都是一个范围内的 commit 的别名, 通常来说, 在调用 log 命令来查看过去的某段时间内, 代码到底发生了什么变化的时候非常的有用:

* **name1..name2** — 这意味着你选中了在树上遍历时, 从 name2 出发, 在遍历到 name1 之前所有可以遍历到的 commit, 当然是不包括 name1 的.如果你在 name1 或者 name2 的位置什么都没写, 那么意味着你希望在相应位置使用 HEAD 来代替 name1/name2.
  * **master..** — 这样可以选中 master 到 HEAD 的所有 commit, 一般来说, 在查看 dev 比 master 快了多少的时候比较有用.
  * **..master** — 这个通常用于: 在你完成了一次 fetch 之后, 想查看自己本地的 master 在上一次 rebase 或者是 merge 之后在远程仓库中被做了什么样的修改.
* **name1...name2** — 中间有三个点的范围和上面说的只有两个点的有很大的差异. 对于 `log` 这类的命令, 三个点的范围选中了所有被 name1 或者 name2 其中之一直接或者间接引用的所有 commit, 注意: 并不包括两者同时引用的 commit. 结果上来说, 这意味着你找到了一系这两个分支并不共同拥有的 commit. 而对于像 `diff` 这样的指令而言, 这样会选中从 name2 到 name1 和 name2 的最近公共祖先之间的所有 commit. `differ` 和 `log` 的区别实际上在于, 从 LCA 到 name1 这条路径上的 commit 并不会被选中.

* **--since="2 weeks ago"** — 这样可以选中某个特定日期之后的所有 commit.
* **--until=”1 week ago”** — 与上一条类似, 选中某个特定日期之前的所有 commit.
* **--grep=pattern** — 选中所有在 commit message 中有和 pattern 匹配的内容的 commit.
* **--committer=pattern** — 选中所有提交者名字和 pattern 相匹配的 commit.
* **--author=pattern** — 选中所有作者名字和 pattern 相匹配的 commit. 作者和提交者是有区别的, 作者是指实际上对目录中的内容做出更改的那个人, 而提交者是将这个更改加入到 repository 中的人. 对于本地仓库来说, 作者和提交者总是一样的, 但是比如说某些更改是通过邮件发送的, 那么作者和提交者往往是不同的.
* **--no-merges** — 顾名思义, 是指选中所有只有一个父 commit 的 commit, 即将所有 merge commit 排除在外.



> Most of these options can be mixed-and-matched. Here is an example which shows the following log entries: changes made to the current branch (branched from master), by myself, within the last month, which contain the text “foo”:

这些选项通常都可以混合使用. 下面这个例子是我选中在当前分支 (这是一个比 master 更快的分支, 比如 dev 分支) 中由我在过去一个月内写的, 并且还在 commit message 中含有 "foo" 的所有 commit:



```bash
$ git log --grep='foo' --author='johnw' --since="1 month ago" master..
```





