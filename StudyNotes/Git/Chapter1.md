# 第一章：安装Git

​		在你开始使用 Git 前，需要将它安装在你的计算机上。 即便已经安装，最好将它升级到最新的版本。 你可以通过软件包或者其它安装程序来安装，或者下载源码编译安装。

## 1. 在 Linux 上安装

​		如果你想在 Linux 上用二进制安装程序来安装 Git，可以使用发行版包含的基础软件包管理工具来安装。 如果以 Fedora 上为例，你可以使用 yum：

```console
  $ sudo yum install git
```

​		如果你在基于 Debian 的发行版上，请尝试用 apt-get：

```console
  $ sudo apt-get install git
```

​		要了解更多选择，Git 官方网站上有在各种 Unix 风格的系统上安装步骤，网址为 http://git-scm.com/download/linux。

## 2. 在 Mac 上安装

​		在 Mac 上安装 Git 有多种方式。 最简单的方法是安装 Xcode Command Line Tools。 Mavericks （10.9） 或更高版本的系统中，在 Terminal 里尝试首次运行 *git* 命令即可。 如果没有安装过命令行开发者工具，将会提示你安装。

​		如果你想安装更新的版本，可以使用二进制安装程序。 官方维护的 OSX Git 安装程序可以在 Git 官方网站下载，网址为 http://git-scm.com/download/mac。

​		你也可以将它作为 GitHub for Mac 的一部分来安装。 它们的图形化 Git 工具有一个安装命令行工具的选项。 你可以从 GitHub for Mac 网站下载该工具，网址为 [http://mac.github.com](http://mac.github.com/)。

## 3. 在 Windows 上安装

​		在 Windows 上安装 Git 也有几种安装方法。 官方版本可以在 Git 官方网站下载。 打开 http://git-scm.com/download/win，下载会自动开始。 要注意这是一个名为 Git for Windows 的项目（也叫做 msysGit），和 Git 是分别独立的项目；更多信息请访问 http://msysgit.github.io/。

​		另一个简单的方法是安装 GitHub for Windows。 该安装程序包含图形化和命令行版本的 Git。 它也能支持 Powershell，提供了稳定的凭证缓存和健全的换行设置。 稍后我们会对这方面有更多了解，现在只要一句话就够了，这些都是你所需要的。 你可以在 GitHub for Windows 网站下载，网址为 [http://windows.github.com](http://windows.github.com/)。

## 4. 从源代码安装

​		有人觉得从源码安装 Git 更实用，因为你能得到最新的版本。 二进制安装程序倾向于有一些滞后，当然近几年 Git 已经成熟，这个差异不再显著。

​		如果你想从源码安装 Git，需要安装 Git 依赖的库：curl、zlib、openssl、expat，还有 libiconv。 如果你的系统上有 yum （如 Fedora）或者 apt-get（如基于 Debian 的系统），可以使用以下命令之一来安装最小化的依赖包来编译和安装 Git 的二进制版：

```console
  $ sudo yum install curl-devel expat-devel gettext-devel \
    openssl-devel zlib-devel
  $ sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
    libz-dev libssl-dev
```

​		为了能够添加更多格式的文档（如 doc, html, info），你需要安装以下的依赖包：

```console
  $ sudo yum install asciidoc xmlto docbook2x
  $ sudo apt-get install asciidoc xmlto docbook2x
```

​		当你安装好所有的必要依赖，你可以继续从几个地方来取得最新发布版本的 tar 包。 你可以从 Kernel.org 网站获取，网址为 https://www.kernel.org/pub/software/scm/git，或从 GitHub 网站上的镜像来获得，网址为 https://github.com/git/git/releases。 通常在 GitHub 上的是最新版本，但 kernel.org 上包含有文件下载签名，如果你想验证下载正确性的话会用到。

​		接着，编译并安装：

```console
  $ tar -zxf git-2.0.0.tar.gz
  $ cd git-2.0.0
  $ make configure
  $ ./configure --prefix=/usr
  $ make all doc info
  $ sudo make install install-doc install-html install-info
```

​		完成后，你可以使用 Git 来获取 Git 的升级：

```console
  $ git clone git://git.kernel.org/pub/scm/git/git.git
```

## 5. Win10下的安装

### 5.1 下载Git

​		搜索Git下载或打开网址https://git-scm.com/downloads

![下载页面](..\images\Git\download_page.png)

​		如上图所示，点击对应系统下载，会自动选取对应系统版本的最新Git版本下载，这里下载的是2.23.0 64Bit版本的。下载完成后得到一个EXE文件，点击安装即可。

### 5.2 安装Git

​		稍作等待会打开一个常见的安装界面，如下图所示：

![安装页面](..\images\Git\set_up.png)

​		然后一路next，首先会经过安装路径选择，如无必要，默认就好；然后是组件安装选择，如无必要，默认就好；之后还有一项默认编辑器（默认Vim，也可以按照自己喜好选择），如之后基本可以按默认选择一路next即可，最后点击instal安装（注：有兴趣可以逐个研究研究）。

​		点击finish完成安装，在桌面点击右键即可发现多了两个右键菜单，Git就按照完成了。正是如图所示，可通过Git GUI或Git Bash的形式使用Git。

![右键菜单](..\images\Git\git_right.png)