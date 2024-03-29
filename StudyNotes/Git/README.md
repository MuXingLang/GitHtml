# Git基础

## 1. 概述

​		Git(读音为/gɪt/。)是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。 Git 是 Linus Torvalds为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

​		分布式相比于集中式的最大区别在于开发者可以提交到本地，每个开发者通过克隆（git clone），在本地机器上拷贝一个完整的Git仓库。

## 2. 简史

​		同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。

​		Linux 内核开源项目有着为数众多的参与者。 绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991－2002年间）。 到 2002 年，整个项目组开始启用一个专有的分布式版本控制系统 BitKeeper 来管理和维护代码。

​		到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。 这就迫使 Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds）基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。 他们对新的系统制订了若干目标：

- 速度

- 简单的设计

- 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）

- 完全分布式

- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

  ​	自诞生于 2005 年以来，Git 日臻成熟完善，在高度易用的同时，仍然保留着初期设定的目标。 它的速度飞快，极其适合管理大项目，有着令人难以置信的非线性分支管理系统。

## 3. 功能特性

从一般开发者的角度来看，git有以下功能：

1、从服务器上克隆完整的Git仓库（包括代码和版本信息）到单机上。

2、在自己的机器上根据不同的开发目的，创建分支，修改代码。

3、在单机上自己创建的分支上提交代码。

4、在单机上合并分支。

5、把服务器上最新版的代码fetch下来，然后跟自己的主分支合并。

6、生成补丁（patch），把补丁发送给主开发者。

7、看主开发者的反馈，如果主开发者发现两个一般开发者之间有冲突（他们之间可以合作解决的冲突），就会要求他们先解决冲突，然后再由其中一个人提交。如果主开发者可以自己解决，或者没有冲突，就通过。

8、一般开发者之间解决冲突的方法，开发者之间可以使用pull 命令解决冲突，解决完冲突之后再向主开发者提交补丁。

从主开发者的角度（假设主开发者不用开发代码）看，git有以下功能：

1、查看邮件或者通过其它方式查看一般开发者的提交状态。

2、打上补丁，解决冲突（可以自己解决，也可以要求开发者之间解决以后再重新提交，如果是开源项目，还要决定哪些补丁有用，哪些不用）。

3、向公共服务器提交结果，然后通知所有开发人员。

## 4. 优缺点

### 4.1 优点

适合分布式开发，强调个体。

公共服务器压力和数据量都不会太大。

速度快、灵活。

任意两个开发者之间可以很容易的解决冲突。

离线工作。

### 4.2 缺点

资料少（起码中文资料很少）。

学习周期相对而言比较长。

不符合常规思维。

代码保密性差，一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息。

## 5. 原理

​		那么，简单地说，Git 究竟是怎样的一个系统呢？ 请注意接下来的内容非常重要，若你理解了 Git 的思想和基本工作原理，用起来就会知其所以然，游刃有余。 在开始学习 Git 的时候，请努力分清你对其它版本管理系统的已有认识，如 Subversion 和 Perforce 等；这么做能帮助你使用工具时避免发生混淆。 Git 在保存和对待各种信息的时候与其它版本控制系统有很大差异，尽管操作起来的命令形式非常相近，理解这些差异将有助于防止你使用中的困惑。

### 5.1 直接记录快照，而非差异比较

​		Git 和其它版本控制系统（包括 Subversion 和近似工具）的主要差别在于 Git 对待数据的方法。 概念上来区分，其它大部分系统以文件变更列表的方式存储信息。 这类系统（CVS、Subversion、Perforce、Bazaar 等等）将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。

![存储每个文件与初始版本的差异](..\images\Git\git_deltas.png)

​		正如上图所示，即存储每个文件与初始版本的差异。

​		Git 不按照以上方式对待或保存数据。 反之，Git 更像是把数据看作是对小型文件系统的一组快照。 每次你提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 **快照流**。

![存储项目随时间改变的快照](..\images\Git\git_snapshots.png)

​		正如上图所示，即存储项目随时间改变的快照。

​		这是 Git 与几乎所有其它版本控制系统的重要区别。 因此 Git 重新考虑了以前每一代版本控制系统延续下来的诸多方面。 Git 更像是一个小型的文件系统，提供了许多以此为基础构建的超强工具，而不只是一个简单的 VCS。 

### 5.2 近乎所有操作都是本地执行

在 Git 中的绝大多数操作都只需要访问本地文件和资源，一般不需要来自网络上其它计算机的信息。 如果你习惯于所有操作都有网络延时开销的集中式版本控制系统，Git 在这方面会让你感到速度之神赐给了 Git 超凡的能量。 因为你在本地磁盘上就有项目的完整历史，所以大部分操作看起来瞬间完成。

举个例子，要浏览项目的历史，Git 不需外连到服务器去获取历史，然后再显示出来——它只需直接从本地数据库中读取。 你能立即看到项目历史。 如果你想查看当前版本与一个月前的版本之间引入的修改，Git 会查找到一个月前的文件做一次本地的差异计算，而不是由远程服务器处理或从远程服务器拉回旧版本文件再来本地处理。

这也意味着你离线或者没有 VPN 时，几乎可以进行任何操作。 如你在飞机或火车上想做些工作，你能愉快地提交，直到有网络连接时再上传。 如你回家后 VPN 客户端不正常，你仍能工作。 使用其它系统，做到如此是不可能或很费力的。 比如，用 Perforce，你没有连接服务器时几乎不能做什么事；用 Subversion 和 CVS，你能修改文件，但不能向数据库提交修改（因为你的本地数据库离线了）。 这看起来不是大问题，但是你可能会惊喜地发现它带来的巨大的不同。

### 5.3 Git 保证完整性

Git 中所有数据在存储前都计算校验和，然后以校验和来引用。 这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。 这个功能建构在 Git 底层，是构成 Git 哲学不可或缺的部分。 若你在传送过程中丢失信息或损坏文件，Git 就能发现。

Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。 这是一个由 40 个十六进制字符（0-9 和 a-f）组成的字符串，基于 Git 中文件的内容或目录结构计算出来。 SHA-1 哈希看起来是这样：

```
24b9da6552252987aa493b52f8696cd6d3b00373
```

Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。 实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

### 5.4 Git 一般只添加数据

你执行的 Git 操作，几乎只往 Git 数据库中增加数据。 很难让 Git 执行任何不可逆操作，或者让它以任何方式清除数据。 同别的 VCS 一样，未提交更新时有可能丢失或弄乱修改的内容；但是一旦你提交快照到 Git 中，就难以再丢失数据，特别是如果你定期的推送数据库到其它仓库的话。

这使得我们使用 Git 成为一个安心愉悦的过程，因为我们深知可以尽情做各种尝试，而没有把事情弄糟的危险。 更深度探讨 Git 如何保存数据及恢复丢失数据的话题，请参考[撤消操作](https://git-scm.com/book/zh/v2/ch00/r_undoing)。

### 5.5 三种状态

好，请注意。 如果你希望后面的学习更顺利，记住下面这些关于 Git 的概念。 Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。 已提交表示数据已经安全的保存在本地数据库中。 已修改表示修改了文件，但还没保存到数据库中。 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。

![工作目录、暂存区域以及 Git 仓库.](..\images\Git\git_areas.png)

​		上图为工作目录、暂存区域以及 Git 仓库。

​		Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

​		工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

​		暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作“索引”，不过一般说法还是叫暂存区域。

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

​		如果 Git 目录中保存着特定版本的文件，就属于已提交状态。 如果作了修改并已放入暂存区域，就属于已暂存状态。 如果自上次取出后，作了修改但还没有放到暂存区域，就是已修改状态。 

## 6. 命令行

​		Git 有多种使用方式。 你可以使用原生的命令行模式，也可以使用 GUI 模式，这些 GUI 软件也能提供多种功能。 在本书中，我们将使用命令行模式。 这是因为首先，只有在命令行模式下你才能执行 Git 的 **所有** 命令，而大多数的 GUI 软件只实现了 Git 所有功能的一个子集以降低操作难度。 如果你学会了在命令行下如何操作，那么你在操作 GUI 软件时应该也不会遇到什么困难，但是，反之则不成立。 此外，由于每个人的想法与侧重点不同，不同的人常常会安装不同的 GUI 软件，但 *所有* 人一定会有命令行工具。

​		假如你是 Mac 用户，我们希望你懂得如何使用终端（Terminal）；假如你是 Windows 用户，我们希望你懂得如何使用命令窗口（Command Prompt）或 PowerShell。 如果你尚未掌握以上技能，我们建议你先停下来快速学习一下，本书中的讲述和举例将用到这些技能。

## 7.在线文档地址

https://git-scm.com/book/zh/v2/

## 8. GitHub GIT CHEAT SHEET

Git is the open source distributed version control system that facilitates GitHub activities on your laptop or
desktop. This cheat sheet summarizes commonly used Git command line instructions for quick reference.

### 8.1 INSTALL GIT

GitHub provides desktop clients that include a graphical user
interface for the most common repository actions and an automatically updating command line edition of Git for advanced scenarios.
[GitHub for Windows]

https://windows.github.com
[GitHub for Mac]
https://mac.github.com
Git distributions for Linux and POSIX systems are available on the
official Git SCM web site.
[Git for All Platforms]
http://git-scm.com

### 8.2 CONFIGURE TOOLING

Configure user information for all local repositories
```bash
$ git config --global user.name "[name]"
```
Sets the name you want atached to your commit transactions
```bash
$ git config --global user.email "[email address]"
```
Sets the email you want atached to your commit transactions
```bash
$ git config --global color.ui auto
```
Enables helpful colorization of command line output
Review edits and craf a commit transaction

### 8.3 CREATE REPOSITORIES

Start a new repository or obtain one from an existing URL
```bash
$ git init [project-name]
```
Creates a new local repository with the specified name
```bash
$ git clone [url]
```
Downloads a project and its entire version history

### 8.4 MAKE CHANGES

Review edits and craf a commit transaction
```bash
$ git status
```
Lists all new or modified files to be commited
```bash
$ git add [file]
```
Snapshots the file in preparation for versioning
```bash
$ git reset [file]
```
Unstages the file, but preserve its contents
```bash
$ git diff
```
Shows file differences not yet staged
```bash
$ git diff --staged
```
Shows file differences between staging and the last file version
```bash
$ git commit -m "[descriptive message]"
```
Records file snapshots permanently in version history

### 8.5 GROUP CHANGES

Name a series of commits and combine completed efforts
```bash
$ git branch
```
Lists all local branches in the current repository
```bash
$ git branch [branch-name]
```
Creates a new branch
```bash
$ git checkout [branch-name]
```
Switches to the specified branch and updates the working directory
```bash
$ git merge [branch]
```
Combines the specified branch’s history into the current branch
```bash
$ git branch -d [branch-name]
```
Deletes the specified branch

### 8.6 REFACTOR FILENAMES

Relocate and remove versioned files
```bash
$ git rm [file]
```
Deletes the file from the working directory and stages the deletion
```bash
$ git rm --cached [file]
```
Removes the file from version control but preserves the file locally
```bash
$ git mv [file-original] [file-renamed]
```
Changes the file name and prepares it for commit

### 8.7 SUPPRESS TRACKING

Exclude temporary files and paths

```
*.log
build/
temp-*
```


A text file named .gitignore suppresses accidental versioning of
files and paths matching the specified paterns

```bash
$ git ls-files --other --ignored --exclude-standard
```

Lists all ignored files in this project

### 8.8 SAVE FRAGMENTS

Shelve and restore incomplete changes
```bash
$ git stash
```
Temporarily stores all modified tracked files
```bash
$ git stash list
```
Lists all stashed changesets
```bash
$ git stash pop
```
Restores the most recently stashed files
```bash
$ git stash drop
```
Discards the most recently stashed changeset

### 8.9 REVIEW HISTORY

Browse and inspect the evolution of project files
```bash
$ git log
```
Lists version history for the current branch
```bash
$ git log --follow [file]
```
Lists version history for a file, including renames
```bash
$ git diff [first-branch]...[second-branch]
```
Shows content differences between two branches
```bash
$ git show [commit]
```
Outputs metadata and content changes of the specified commit

### 8.10 REDO COMMITS

Erase mistakes and craf replacement history
```bash
$ git reset [commit]
```
Undoes all commits afer [commit], preserving changes locally
```bash
$ git reset --hard [commit]
```
Discards all history and changes back to the specified commit

### 8.11 SYNCHRONIZE CHANGES

Register a repository bookmark and exchange version history
```bash
$ git fetch [bookmark]
```
Downloads all history from the repository bookmark
```bash
$ git merge [bookmark]/[branch]
```
Combines bookmark’s branch into current local branch
```bash
$ git push [alias] [branch]
```
Uploads all local branch commits to GitHub
```bash
$ git pull
```
Downloads bookmark history and incorporates changes

## 9. 参考内容

《Pro Git》


