# 附录B：将Git嵌入你的系统

​		假设你的应用程序的目标人群是开发者，如果它能够被整合进一些源码控制的功能，那真真是极好的。 甚至对于一个例如文档编辑器之类的不是为开发者而设计的应用程序，它们也可能从版本控制系统中受益，并且 Git 的实现方式在很多情况下都表现得非常出色。

​		如果你想将 Git 整合进你的应用程序的话，一般来说你有三种可能的选择：启动一个 shell 来使用 Git 的命令行工具；使用 Libgit2；或者使用 JGit。

## 1. 命令行 Git 方式

​		一种方式就是启动一个 shell 进程并在里面使用 Git 的命令行工具来完成任务。 这种方式看起来很循规蹈矩，但是它的优点也因此而来，就是支持所有的 Git 的特性。 它也碰巧相当简单，因为几乎所有运行时环境都有一个相对简单的方式来调用一个带有命令行参数的进程。 然而，这种方式也有一些固有的缺点。

​		一个就是所有的输出都是纯文本格式。 这意味着你将被迫解析 Git 的有时会改变的输出格式，以随时了解它工作的进度和结果。更糟糕的是，这可能是无效率并且容易出错的。

​		另外一个就是令人捉急的错误修复能力。 如果一个版本库被莫名其妙地损毁，或者用户使用了一个奇奇怪怪的配置， Git 只会简单地拒绝表现自己的强大能力。

​		还有一个就是进程的管理。 Git 会要求你在一个独立的进程中维护一个 shell 环境，这可能会无谓地增加复杂性。 试图协调许许多多的类似的进程（尤其是在某些情况下，当不同的进程在访问相同的版本库时）是对你的能力的极大挑战。

## 2. Libgit2

​		© 另外一种可以供你使用的是 Libgit2。 Libgit2 是一个 Git 的非依赖性的工具，它致力于为其他程序使用 Git 提供更好的 API。 你可以在 [http://libgit2.github.com](http://libgit2.github.com/) 找到它。

​		首先，让我们来看一下 C API 长啥样。 这是一个旋风式旅行。

```c
// 打开一个版本库
git_repository *repo;
int error = git_repository_open(&repo, "/path/to/repository");

// 逆向引用 HEAD 到一个提交
git_object *head_commit;
error = git_revparse_single(&head_commit, repo, "HEAD^{commit}");
git_commit *commit = (git_commit*)head_commit;

// 显示这个提交的一些详情
printf("%s", git_commit_message(commit));
const git_signature *author = git_commit_author(commit);
printf("%s <%s>\n", author->name, author->email);
const git_oid *tree_id = git_commit_tree_id(commit);

// 清理现场
git_commit_free(commit);
git_repository_free(repo);
```

​		前两行打开一个 Git 版本库。 这个 `git_repository` 类型代表了一个在内存中带有缓存的指向一个版本库的句柄。 这是最简单的方法，只是你必须知道一个版本库的工作目录或者一个 `.git` 文件夹的精确路径。 另外还有 `git_repository_open_ext` ，它包括了带选项的搜索， `git_clone` 及其同类可以用来做远程版本库的本地克隆， `git_repository_init` 则可以创建一个全新的版本库。

​		第二段代码使用了一种 rev-parse 语法（要了解更多，请看 [分支引用]）来得到 HEAD 真正指向的提交。 返回类型是一个 `git_object` 指针，它指代位于版本库里的 Git 对象数据库中的某个东西。 `git_object` 实际上是几种不同的对象的“父”类型，每个“子”类型的内存布局和 `git_object` 是一样的，所以你能安全地把它们转换为正确的类型。 在上面的例子中， `git_object_type(commit)` 会返回 `GIT_OBJ_COMMIT` ，所以转换成 `git_commit` 指针是安全的。

​	下一段展示了如何访问一个提交的详情。 最后一行使用了 `git_oid` 类型，这是 Libgit2 用来表示一个 SHA-1 哈希的方法。

从这个例子中，我们可以看到一些模式：

- 如果你声明了一个指针，并在一个 Libgit2 调用中传递一个引用，那么这个调用可能返回一个 int 类型的错误码。 值 `0` 表示成功，比它小的则是一个错误。
- 如果 Libgit2 为你填入一个指针，那么你有责任释放它。
- 如果 Libgit2 在一个调用中返回一个 `const` 指针，你不需要释放它，但是当它所指向的对象被释放时它将不可用。
- 用 C 来写有点蛋疼。

​		最后一点意味着你应该不会在使用 Libgit2 时编写 C 语言程序。 但幸运的是，有许多可用的各种语言的绑定，能让你在特定的语言和环境中更加容易的操作 Git 版本库。 我们来看一下下面这个用 Libgit2 的 Ruby 绑定写成的例子，它叫 Rugged，你可以在 https://github.com/libgit2/rugged 找到它。

```ruby
repo = Rugged::Repository.new('path/to/repository')
commit = repo.head.target
puts commit.message
puts "#{commit.author[:name]} <#{commit.author[:email]}>"
tree = commit.tree
```

​		你可以发现，代码看起来更加清晰了。 首先， Rugged 使用异常机制，它可以抛出类似于 `ConfigError` 或者 `ObjectError` 之类的东西来告知错误的情况。 其次，不需要明确资源释放，因为 Ruby 是支持垃圾回收的。 我们来看一个稍微复杂一点的例子：从头开始制作一个提交。

```ruby
blob_id = repo.write("Blob contents", :blob) (1)

index = repo.index
index.read_tree(repo.head.target.tree)
index.add(:path => 'newfile.txt', :oid => blob_id) (2)

sig = {
    :email => "bob@example.com",
    :name => "Bob User",
    :time => Time.now,
}

commit_id = Rugged::Commit.create(repo,
    :tree => index.write_tree(repo), (3)
    :author => sig,
    :committer => sig, (4)
    :message => "Add newfile.txt", (5)
    :parents => repo.empty? ? [] : [ repo.head.target ].compact, (6)
    :update_ref => 'HEAD', (7)
)
commit = repo.lookup(commit_id) (8)
```

1. 创建一个新的 blob ，它包含了一个新文件的内容。
2. 将 HEAD 提交树填入索引，并在路径 `newfile.txt` 增加新文件。
3. 这就在 ODB 中创建了一个新的树，并在一个新的提交中使用它。
4. 我们在 author 栏和 committer 栏使用相同的签名。
5. 提交的信息。
6. 当创建一个提交时，你必须指定这个新提交的父提交。 这里使用了 HEAD 的末尾作为单一的父提交。
7. 在做一个提交的过程中， Rugged （和 Libgit2 ）能在需要时更新引用。
8. 返回值是一个新提交对象的 SHA-1 哈希，你可以用它来获得一个 `Commit` 对象。

​		Ruby 的代码很好很简洁，另一方面因为 Libgit2 做了大量工作，所以代码运行起来其实速度也不赖。 如果你不是一个 Ruby 程序员，我们在 [其它绑定]有提到其它的一些绑定。

### 2.1 高级功能

​		Libgit2 有几个超过核心 Git 的能力。 例如它的可定制性：Libgit2 允许你为一些不同类型的操作自定义的“后端”，让你得以使用与原生 Git 不同的方式存储东西。 Libgit2 允许为自定义后端指定配置、引用的存储以及对象数据库，

​		我们来看一下它究竟是怎么工作的。 下面的例子借用自 Libgit2 团队提供的后端样本集 （可以在 https://github.com/libgit2/libgit2-backends 上找到）。 一个对象数据库的自定义后端是这样建立的：

```c
git_odb *odb;
int error = git_odb_new(&odb); (1)

git_odb_backend *my_backend;
error = git_odb_backend_mine(&my_backend, /*…*/); (2)

error = git_odb_add_backend(odb, my_backend, 1); (3)

git_repository *repo;
error = git_repository_open(&repo, "some-path");
error = git_repository_set_odb(odb); (4)
```

*（注意：这个错误被捕获了，但是没有被处理。我们希望你的代码比我们的更好。）*

1. 初始化一个空的对象数据库（ ODB ）“前端”，它将被作为一个用来做真正的工作的“后端”的容器。
2. 初始化一个自定义 ODB 后端。
3. 为这个前端增加一个后端。
4. 打开一个版本库，并让它使用我们的 ODB 来寻找对象。

​		但是 `git_odb_backend_mine` 是个什么东西呢？ 嗯，那是一个你自己的 ODB 实现的构造器，并且你能在那里做任何你想做的事，前提是你能正确地填写 `git_odb_backend` 结构。 它看起来_应该_是这样的：

```c
typedef struct {
    git_odb_backend parent;

    // 其它的一些东西
    void *custom_context;
} my_backend_struct;

int git_odb_backend_mine(git_odb_backend **backend_out, /*…*/)
{
    my_backend_struct *backend;

    backend = calloc(1, sizeof (my_backend_struct));

    backend->custom_context = …;

    backend->parent.read = &my_backend__read;
    backend->parent.read_prefix = &my_backend__read_prefix;
    backend->parent.read_header = &my_backend__read_header;
    // ……

    *backend_out = (git_odb_backend *) backend;

    return GIT_SUCCESS;
}
```

​		`my_backend_struct` 的第一个成员必须是一个 `git_odb_backend` 结构，这是一个微妙的限制：这样就能确保内存布局是 Libgit2 的代码所期望的样子。 其余都是随意的，这个结构的大小可以随心所欲。

​		这个初始化函数为该结构分配内存，设置自定义的上下文，然后填写它支持的 `parent` 结构的成员。 阅读 Libgit2 的 `include/git2/sys/odb_backend.h` 源码以了解全部调用签名，你特定的使用环境会帮你决定使用哪一种调用签名。

### 2.2 其它绑定

​		Libgit2 有很多种语言的绑定。 在这篇文章中，我们展现了一个使用了几个更加完整的绑定包的小例子，这些库存在于许多种语言中，包括 C++、Go、Node.js、Erlang 以及 JVM ，它们的成熟度各不相同。 官方的绑定集合可以通过浏览这个版本库得到：https://github.com/libgit2[] 。 我们写的代码将返回当前 HEAD 指向的提交的提交信息（就像 `git log -1` 那样）。

#### 2.2.1 LibGit2Sharp

​		如果你在编写一个 .NET 或者 Mono 应用，那么 LibGit2Sharp（https://github.com/libgit2/libgit2sharp[]） 就是你所需要的。 这个绑定是用 C# 写成的，并且已经采取许多措施来用令人感到自然的 CLR API 包装原始的 Libgit2 的调用。 我们的例子看起来就像这样：

```csharp
new Repository(@"C:\path\to\repo").Head.Tip.Message;
```

​		对于 Windows 桌面应用，一个叫做 NuGet 的包会让你快速上手。

#### 2.2.2 objective-git

​		如果你的应用运行在一个 Apple 平台上，你很有可能使用 Objective-C 作为实现语言。 Objective-Git (https://github.com/libgit2/objective-git) 是这个环境下的 Libgit2 绑定。 一个例子看起来类似这样：

```objc
GTRepository *repo =
    [[GTRepository alloc] initWithURL:[NSURL fileURLWithPath: @"/path/to/repo"] error:NULL];
NSString *msg = [[[repo headReferenceWithError:NULL] resolvedTarget] message];
```

​		Objective-git 与 Swift 完美兼容，所以你把 Objective-C 落在一边的时候不用恐惧。

#### 2.2.3 pygit2

​		Python 的 Libgit2 绑定叫做 Pygit2 ，你可以在 http://www.pygit2.org/ 找到它。 我们的示例程序：

```python
pygit2.Repository("/path/to/repo") # 打开版本库
    .head                          # get the current branch
    .peel(pygit2.Commit)           # walk down to the commit
    .message                       # read the message
```

### 2.3 扩展阅读

​		当然，完全阐述 Libgit2 的能力已超出本书范围。 如果你想了解更多关于 Libgit2 的信息，可以浏览它的 API 文档： https://libgit2.github.com/libgit2, 以及一系列的指南： https://libgit2.github.com/docs. 对于其它的绑定，检查附带的 README 和测试文件，那里通常有简易教程，以及指向拓展阅读的链接。

## 3. JGit

​		如果你想在一个 Java 程序中使用 Git ，有一个功能齐全的 Git 库，那就是 JGit 。 JGit 是一个用 Java 写成的功能相对健全的 Git 的实现，它在 Java 社区中被广泛使用。 JGit 项目由 Eclipse 维护，它的主页在 http://www.eclipse.org/jgit 。

### 3.1 起步

​		有很多种方式可以让 JGit 连接你的项目，并依靠它去写代码。 最简单的方式也许就是使用 Maven 。你可以通过在你的 pom.xml 文件里的 `<dependencies>` 标签中增加像下面这样的片段来完成这个整合。

```xml
<dependency>
    <groupId>org.eclipse.jgit</groupId>
    <artifactId>org.eclipse.jgit</artifactId>
    <version>3.5.0.201409260305-r</version>
</dependency>
```

​		在你读到这段文字时 `version` 很可能已经更新了，所以请浏览 http://mvnrepository.com/artifact/org.eclipse.jgit/org.eclipse.jgit 以获取最新的仓库信息。 当这一步完成之后， Maven 就会自动获取并使用你所需要的 JGit 库。

​		如果你想自己管理二进制的依赖包，那么你可以从 http://www.eclipse.org/jgit/download 获得预构建的 JGit 二进制文件。 你可以像下面这样执行一个命令来将它们构建进你的项目。

```console
javac -cp .:org.eclipse.jgit-3.5.0.201409260305-r.jar App.java
java -cp .:org.eclipse.jgit-3.5.0.201409260305-r.jar App
```

### 3.2 底层命令

​		JGit 的 API 有两种基本的层次：底层命令和高层命令。 这个两个术语都来自 Git ，并且 JGit 也被按照相同的方式粗略地划分：高层 API 是一个面向普通用户级别功能的友好的前端（一系列普通用户使用 Git 命令行工具时可能用到的东西），底层 API 则直接作用于低级的仓库对象。

​		大多数 JGit 会话会以 `Repository` 类作为起点，你首先要做的事就是创建一个它的实例。 对于一个基于文件系统的仓库来说（嗯， JGit 允许其它的存储模型），用 `FileRepositoryBuilder` 完成它。

```java
// 创建一个新仓库
Repository newlyCreatedRepo = FileRepositoryBuilder.create(
    new File("/tmp/new_repo/.git"));
newlyCreatedRepo.create();

// 打开一个存在的仓库
Repository existingRepo = new FileRepositoryBuilder()
    .setGitDir(new File("my_repo/.git"))
    .build();
```

​		无论你的程序是否知道仓库的确切位置，builder 中的那个流畅的 API 都可以提供给它寻找仓库所需所有信息。 它可以使用环境变量 （`.readEnvironment()`） ，从工作目录的某处开始并搜索 （`.setWorkTree(…).findGitDir()`） , 或者仅仅只是像上面那样打开一个已知的 `.git` 目录。

​		当你拥有一个 `Repository` 实例后，你就能对它做各种各样的事。 下面是一个速览：

```java
// 获取引用
Ref master = repo.getRef("master");

// 获取该引用所指向的对象
ObjectId masterTip = master.getObjectId();

// Rev-parse
ObjectId obj = repo.resolve("HEAD^{tree}");

// 装载对象原始内容
ObjectLoader loader = repo.open(masterTip);
loader.copyTo(System.out);

// 创建分支
RefUpdate createBranch1 = repo.updateRef("refs/heads/branch1");
createBranch1.setNewObjectId(masterTip);
createBranch1.update();

// 删除分支
RefUpdate deleteBranch1 = repo.updateRef("refs/heads/branch1");
deleteBranch1.setForceUpdate(true);
deleteBranch1.delete();

// 配置
Config cfg = repo.getConfig();
String name = cfg.getString("user", null, "name");
```

​		这里完成了一大堆事情，所以我们还是一次理解一段的好。

​		第一行获取一个指向 `master` 引用的指针。 JGit 自动抓取位于 `refs/heads/master` 的 *真正的* master 引用，并返回一个允许你获取该引用的信息的对象。 你可以获取它的名字 （`.getName()`） ，或者一个直接引用的目标对象 （`.getObjectId()`） ，或者一个指向该引用的符号指针 （`.getTarget()`） 。 引用对象也经常被用来表示标签的引用和对象，所以你可以询问某个标签是否被“削除”了，或者说它指向一个标签对象的（也许很长的）字符串的最终目标。

​		第二行获得以 `master` 引用的目标，它返回一个 ObjectId 实例。 不管是否存在于一个 Git 对象的数据库，ObjectId 都会代表一个对象的 SHA-1 哈希。 第三行与此相似，但是它展示了 JGit 如何处理 rev-parse 语法（要了解更多，请看 [分支引用]），你可以传入任何 Git 了解的对象说明符，然后 JGit 会返回该对象的一个有效的 ObjectId ，或者 `null` 。

​		接下来两行展示了如何装载一个对象的原始内容。 在这个例子中，我们调用 `ObjectLoader.copyTo()` 直接向标准输出流输出对象的内容，除此之外 ObjectLoader 还带有读取对象的类型和长度并将它以字节数组返回的方法。 对于一个（ `.isLarge()` 返回 `true` 的）大的对象，你可以调用 `.openStream()` 来获得一个类似 InputStream 的对象，它可以在没有一次性将所有数据拉到内存的前提下读取对象的原始数据。

​		接下来几行展现了如何创建一个新的分支。 我们创建一个 RefUpdate 实例，配置一些参数，然后调用 `.update()` 来确认这个更改。 删除相同分支的代码就在这行下面。 记住必须先 `.setForceUpdate(true)` 才能让它工作，否则调用 `.delete()` 只会返回 `REJECTED` ，然后什么都没有发生。

​		最后一个例子展示了如何从 Git 配置文件中获取 `user.name` 的值。 这个 Config 实例使用我们先前打开的仓库做本地配置，但是它也会自动地检测并读取全局和系统的配置文件。

​		这只是底层 API 的冰山一角，另外还有许多可以使用的方法和类。 还有一个没有放在这里说明的，就是 JGit 是用异常机制来处理错误的。 JGit API 有时使用标准的 Java 异常（例如 `IOException` ），但是它也提供了大量 JGit 自己定义的异常类型（例如 `NoRemoteRepositoryException`、 `CorruptObjectException` 和 `NoMergeBaseException`）。

### 3.3 高层命令

​		底层 API 更加完善，但是有时将它们串起来以实现普通的目的非常困难，例如将一个文件添加到索引，或者创建一个新的提交。 为了解决这个问题， JGit 提供了一系列高层 API ，使用这些 API 的入口点就是 `Git` 类：

```java
Repository repo;
// 构建仓库。。。
Git git = new Git(repo);
```

​		Git 类有一系列非常好的 *构建器* 风格的高层方法，它可以用来构造一些复杂的行为。 我们来看一个例子——做一件类似 `git ls-remote` 的事。

```java
CredentialsProvider cp = new UsernamePasswordCredentialsProvider("username", "p4ssw0rd");
Collection<Ref> remoteRefs = git.lsRemote()
    .setCredentialsProvider(cp)
    .setRemote("origin")
    .setTags(true)
    .setHeads(false)
    .call();
for (Ref ref : remoteRefs) {
    System.out.println(ref.getName() + " -> " + ref.getObjectId().name());
}
```

​		这是一个 Git 类的公共样式，这个方法返回一个可以让你串连若干方法调用来设置参数的命令对象，当你调用 `.call()` 时它们就会被执行。 在这情况下，我们只是请求了 `origin` 远程的标签，而不是头部。 还要注意用于验证的 `CredentialsProvider` 对象的使用。

​		在 Git 类中还可以使用许多其它的命令，包括但不限于 `add`、`blame`、`commit`、`clean`、`push`、`rebase`、`revert` 和 `reset`。

### 3.4 拓展阅读

​		这只是 JGit 的全部能力的冰山一角。 如果你对这有兴趣并且想深入学习，在下面可以找到一些信息和灵感。

- JGit API 在线官方文档： http://download.eclipse.org/jgit/docs/latest/apidocs 。 这是基本的 Javadoc ，所以你也可以在你最喜欢的 JVM IDE 上将它们安装它们到本地。
- JGit Cookbook ： https://github.com/centic9/jgit-cookbook 拥有许多如何利用 JGit 实现特定任务的例子。
- http://stackoverflow.com/questions/6861881 指出了几个好的资源。