# Git 协同与提交规范

<a name="df368884"></a>
## 前言
在现代化的软件开发中，尤其是在企业中的团队协同开发流程与代码管理方面，git 都充当着不可或缺的角色。本章中将会讲述如何使用 git 来进行一些基本的版本控制操作与团队协同中的 git 使用流程管理与提交规范。<br />在 Git 快速上手章节中你将会了解 git 的基本概念与一些常用命令，能让你快速上手或复习 git 的基本操作。<br />在 Git 多人合作基本模型中，你将会了解到 git 的团队基本工作模型。<br />Git 工作流与分支管理规范是对于在企业级中 git 的团队协同流程的介绍与规范的讲解，你会学到团队开发中 git 在软件开发生命周期中的角色。<br />Git 提交规范总结了在 git 使用过程中对于代码记录的提交信息的规范要求。

<a name="9dbb5bb4"></a>
## Git 快速上手
<a name="8f814b45"></a>
### Git 的简单介绍
Git 是一个用于控制管理代码历史版本的工具，区别于集中式的 SVN，Git 使用了分布式的管理方式。<br />![image.png](/1.png)<br />									    _(Git 的分布式版本管理)_<br />团队每人都拥有一个自己的本地仓库，不仅可以自己本地进行版本管理，也可以将代码版本在各个成员间共享。
<a name="1d2b5cb6"></a>
### 初始化一个本地仓库
在开始操作之前，确保你的系统环境已经安装了 Git， [https://git-scm.com](https://git-scm.com) 上提供了各个系统环境的 Git 下载。<br />在使用 git 命令进行操作之前，你需要一个 git 仓库，你可以移动到你的工作目录，命令行中执行:

```shell
$ git init
Initialized empty Git repository in /path/to/work/directory/.git/
```

这样你就可以得到一个本地仓库了，你会看到工作目录下已经新增了一个 _.git_ 目录。

_注：某些系统中可能预设隐藏以 . 开头的文件夹，可能需要开启相关设置才能看到_
<a name="fc8bc6e2"></a>
### 克隆远程仓库
当然如果你已经有了一个远程仓库，你就不需要初始化一个本地仓库了，你可以使用：

```shell
$ git clone <remote address>
```

上面的命令会将链接指向的远程仓库克隆到本地，这样你也得到了一个 _git_ 仓库，并且这个仓库是与远程仓库关联的，如果你有远程仓库的权限，你就可以对这个仓库推送你的提交。
<a name="308886f8"></a>
### 查看仓库状态
上面我们已经初始化了一个空仓库，我们可以通过指令：

```shell
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

通过 _git status_ 查看当前的仓库(或代码分支)的状态。
<a name="be28fbd9"></a>
### 追踪文件修改
接下来我们可以为这个仓库添加一些文件, 在工作目录下执行：

```shell
$ touch index.js
```

上面的指令为我们在工作目录下创建一个名为 index.js 的文件, 使用 git status 可以查看到已经添加了一个 index.js 文件, 下面我们在这个文件中写入一些内容：

```shell
$ echo "console.log('learning git!');" > index.js
```

上面这条指令为我们将 _"console.log('learning git!')"_ 这个内容写入了 _index.js_ 文件中。此时查看仓库状态如下：

```shell
$ git status
On branch master

No commits yet

Untracked files:
    (use "git add <file>..." to include in what will be commited)
    
        index.js
nothing added to commit but untracked files present(use "git add" to track)
```

可以看到信息反馈说到 index.js 这个文件是未追踪的(_Untracked files_), 而在 git 中，如果一个文件需要 git 进行版本控制与内容修改追踪，那么就需要先将它加入到 git 仓库的暂存区 (_stage_) 中。将文件加入到暂存区只需要执行：

```shell
$ git add index.js
```

![image.png](/2.png)<br />							   _(将工作区的修改通过 add 命令移到暂存区)_

相信很多人在使用 git 添加修改或者新增文件的时候，会习惯直接使用

```shell
$ git add .
```

直接将当前目录下的修改的全部文件添加到暂存区(_stage_), 一般情况下，这样的方式既方便又快捷，但是我仍然推荐在执行 _git add_ 先使用 _git status_ 命令来查看一下仓库有哪些文件被修改了，这样是因为如果你的仓库中使用了 git 的子模块功能，直接 _git add ._ 会将子模块的 commit ID 也一并添加到暂存区，万一这个 commit ID 的提交并不是你的本意，那么提交到远程可能会导致子模块错乱的问题。<br />为了养成良好习惯，最好在 _git add_ 前使用 _git status_ 查看一下仓库状态。
<a name="2cffc01d"></a>
### 提交文件修改的历史信息
通过 git add 我们已经将对文件的操作(新增或修改, 删除等)添加到了暂存区 (_stage_) 中，但是这样是不够的，我们还需要对这个操作记录成一个历史记录：

```shell
$ git commit -m "Add a file named index.js"
[master (root-commit) 910f65f] Add a file named index.js

1 file changed, 1 insertion(+)
create mode 100644 index.js
```

这样我们对文件的操作生成一条历史记录，历史记录可以通过 _git log_ 进行查看。不加 -m 参数直接 commit 的提交信息会默认使用 vim 编辑器进行编辑(对于 vim 编辑器的操作介绍请看后面章节)。对于 commit 记录，原则上我们应该每个 commit 所记录的修改保持纯洁与单一，不允许比如对两个 bug 的修复都提交到一个 commit 中的情况，这样一旦 bug A 的修复会导致另一个问题，但是 bug B 的修复是没有问题的，此时如果回退该 commit，虽然回退了 bug A 的提交，但是也丢失 bug B 的修复工作了。<br />![image.png](/3.png)<br />							_(将暂存区的修改通过 commit 命令存储到历史库中)_<br />_注：笔者不推荐 commit -m 的操作，因为这不符合提交规范，这里是为了简短说明 commit 的使用，详情请看后面章节_
<a name="5174288b"></a>
#### Vim 编辑器的简单介绍
使用 git commit 命令会默认使用 vim 编辑器进行提交信息的编写，同时笔者不推荐使用 git commit -m 的方式来提交信息，因为这样编写出来的提交信息不符合规范，只能说明一些简短信息或者甚至直接为空，尤其是空信息是不被允许的。所以这里会简单介绍 vim 的基本用法，有兴趣的读者可以搜索相关 vim 的学习资料进行深入学习。<br />当你使用 add 命令将对于文件的修改添加到缓存区后，你可以使用输入 git commit 然后直接回车，然后你会进入到 vim 编辑器的界面，你会在界面上看到类似下面的信息：

```
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Committer: shawn <zhangxiang@zhangxiangdeMacBook-Pro.local>
#
# On branch master
# Changes to be committed:
#       modified:   index.js
#
~
~
"~/path/to/working/directory/git-learning/.git/COMMIT_EDITMSG" 10L, 274C
```

以 _#_ 开头的信息是注释，不用担心他们会添加到你的提交信息中，而且如果你没有填写信息就直接退出则该 commit 会被中断。
<a name="145d9a83"></a>
##### 插入模式
接下来，当你需要填写数据的时候，你需要按下 "i" 键进入插入模式，进入插入模式之后才能进行文本的插入与编写，然后就可以类似在一般的编辑器下编写你想填写的信息。<br />而当你编写完信息后，你需要先退出插入模式而回到原来的普通模式，退出插入模式你只需要按下 "esc" 键。
<a name="a5c7aeff"></a>
##### 命令模式
当你想退出 vim 编辑器的时候，我们需要进入命令模式，在普通模式下输入 **:** ， 进入命令模式，输入 _wq_ 回车,就可以保存并退出编辑了，下面提供一些常用的命令供大家参考：

```
:q   退出
:q!  强制退出，不进行保存
:wq  保存并退出
:wq! 强制保存并退出
```

<a name="02a9bea5"></a>
### 使用分支
分支功能是 git 最为强大的功能之一，它能够让你并发地在多个场景下进行开发。并且可以让你同时开发不同功能而不冲突，用于区分功能或版本。<br />在企业里面，有规模的开发团队在开发功能的时候是不可能大家都在一个分支上开发的，因为这可能需要不断地解决冲突，此外功能与功能间通常相互独立，在同一个分支上开发本就不合理。<br />你可以执行下面这个命令来创建一个分支：

```shell
$ git branch <branch-name>
```

例如你执行 _git branch test_ 这条命令创建了一条分支，实际上你是创建了一个指向该 commit 节点的名为 test 的指针：<br />![image.png](/4.png)<br />									_(创建一个名为 test 的分支)_
<a name="cca48627"></a>
### 在不同场景间切换
在日常开发的时候，我们可能经常会需要切换到不同的开发场景，比如你正在一个功能分支上开发，但是产品经理或者测试忽然需要你在线上环境看一个问题，所以你需要将你目前的代码切换到线上环境的代码，当你需要切换分支的时候，可以：

```shell
$ git checkout <branch-name>|<tag>|<commit hash id>
```

上面只是 git checkout 的使用场景之一，checkout 不仅可以切换分支，还可以切换 tag，或者 commit hash id 等环境的代码。比如你需要切换到上面创建的 test 分支，那么你需要执行 _git checkout test_ 这条指令，在 git 中是使用 Head 指针来指向当前提交记录的，而 checkout 命令实际上是将 git 的 head 指针指向了 test 这一指针：<br />![image.png](/5.png)<br />						_(将当前工作分支由 master 分支变为 test 分支)_

那么现在的当前工作分支就是 test 分支了。这里就会明白，实际上 checkout 命令的本质就是移动 git 的 Head 指针。<br />当然，直接 checkout 可能会不能顺利切换过去，因为你的工作区内对于某些文件的修改没有提交，会 git 阻止你进行分支切换，提醒你先对文件的修改进行 commit 提交，相关的 commit 提交操作像上面的章节一样操作就可以，但是如果你因为一些其他原因不希望进行 commit 比如这部分的修改不满足一个 commit 或者这些是实验性的代码，所以你可以将当前的工作区修改进行 "储藏"：

```shell
$ git stash
```

git stash 命令可以为你保存起目前的代码，并将这些修改压入到一个栈中，换言之你可以多次进行 stash。<br />然后当你解决了问题，想回到原来的工作代码环境，你只需要切换到原来的分支上，进行：

```shell
$ git stash pop
```

这里会默认拿栈顶的代码(也就是最近一次 stash 的代码)修改记录并将其反映到你的工作区中。

<a name="acfe1154"></a>
## Git 多人合作基本模型

在实际的软件研发过程中，团队作战的场景远远多于单兵作战，所以仅仅是掌握上面章节对于自己本地仓库进行操作的 git 指令是不够的，我们会需要一个远程的集中式仓库。所谓集中式仓库就是团队中所有人的代码都上传到这个地方进行存储，有了集中式的仓库，可以方便地进行成果共享与协作。

<a name="62d26794"></a>
### 添加远程仓库地址
如果你已经使用 _git init_ 初始化了一个 git 仓库，并且你想要将你的工作成果放到一个远程仓库上，为可能的多人协作做准备的话，你需要为你的本地仓库添加远程连接，也即是说将你本地仓库的历史库放到一个远程服务器的仓库中保存里面：

```shell
$ git remote add <name> <remote address>
```

使用 _git remote add_ 命令为本地仓库添加一个远程仓库的地址，地址可以是 _http/https_ 格式的也可以是 _ssh_ 格式的。而 name 是指这个远程仓库链接名称，比如：

```shell
$ git remote add origin https://github.com/username/example
```

当然，如果你的仓库是通过 _git clone_ 克隆下来的，你可以不需要添加一个远程仓库链接。
<a name="d41d8cd9"></a>
### 
<a name="bda0b674"></a>
### 将本地修改推送到远程仓库
当你有若干个 _commit_ 提交记录需要更新到远程仓库，在执行了 _commit_ 命令后，使用：

```shell
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 274 bytes | 274.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/username/example.git
 * [new branch]      master -> master
```

这样就可以将你本地的 _commit_ 提交传送到远程仓库中进行共享。注意第一个推送修改的时候需要指定远程连接的名称(这里是 origin, 即上一步添加远程仓库地址的名称), 并指定推送的分支名(这里是 master 分支)。

<a name="b60f5c25"></a>
### 拉取并合并他人修改
当团队其他成员对该分支上的代码进行了修改并且已经 _push_ 到远程仓库后，你可以将他的提交拉取到本地并且将他的提交进行合并自己本地仓库上，通过：

```shell
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master
```

第一次拉取远程仓库的修改的时候，会出现像上面 _"There is no tracking information for the current branch"_ 的错误，这是因为本地分支与远程分支没有建立起关联关系，所以我们需要通过以下两种方式来达到目的:

1. 通过执行 _pull_ 命令的时候指定分支<br />

```shell
$ git pull origin master
```


2. 通过执行 _branch_ 命令建立本地分支与远程分支的关联关系后再次执行 _pull_ 命令 <br />

```shell
$ git branch --set-upstream-to=origin/master
Branch 'master' set up to track remote branch 'master' from 'origin'.
$ git pull
```

这样，你本地仓库就会出现其他人提交的代码修改了。

<a name="c1185d41"></a>
### 协作基本模型
使用 git 的 push 命令与 pull 命令其实已经可以实现多人代码共享的情景，你对本地代码进行了很多修改，同时创建了很多 commit 记录，然后你可以通过 push 推送到远程仓库，团队其他人通过 pull 命令拉取你的提交，同时他们也有可能将他们的 commit 记录提交远程仓库，你可以使用 pull 命令拉取他们的提交。<br />![image.png](/6.png)<br />										_(团队基本协作模型)_<br />_<br />但是大家并行开发同时进行提交，大概率会出现 push 的时候被 git 阻止的情况，原因是两个并行的提交，远程仓库需要对两者的提交顺序进行编排，所以会要求你先进行 pull 命令执行，将远程的提交拉取下来并合并到本地分支，在本地编排好你的本地提交记录与其他人的提交记录之后，才允许你进行 push 命令的执行。

<a name="837fe4a5"></a>
## Git 工作流与分支管理规范
对于一些规模比较大的团队，一般会使用 git 分支来管理与规范不同环境的代码。
<a name="7e4ed868"></a>
### 分支管理的必要性
也许你会有疑问为什么我们会需要分支管理，并且需要分如此多的分支？因为在软件开发中，我们通常会有几个阶段，比如开发阶段，冒烟阶段，提测阶段，预发布阶段与上线阶段，而这些阶段都会对应有不同的部署环境，如果我们没有分支管理与将这些环境分别用不同的分支进行区分，代码版本管理将会无从谈起。所以我们会给分出很多分支，并且赋予这些分支不同的角色对应不同的环境与阶段。<br />_注：1. 冒烟这一术语原来自硬件行业，在软件工程中指由开发人员自身对新的程序代码进行测试，确保新的代码不出故障_
<a name="6addf7c8"></a>
### 使用 gitflow 工作流
对于分支的管理，推荐使用 gitflow 工作流的模式，流程大概如下图：<br />![image.png](/7.png)<br />							 		_(gitflow 分支管理)_<br />对上面的分支说明如下：

- feature branches。功能分支，一般是从 develop 开发分支上检出(checkout)<br />
- develop。公共的开发主线分支，feature 功能分支的代码开发完成后，经过 code review 后会合并到此分支<br />
- release branches。测试、发布主线，此分支是从 develop 分支上检出(checkout), 一般是提测阶段会使用该分支的代码<br />
- bugfix。修复 release 分支问题，<br />
- hotfix。紧急修复，一般是用于修复上线后的生产环境的问题。<br />
- master。可发布的稳定版分支<br />

_注：gitflow 分支管理图来自网络_
<a name="884fe4ba"></a>
#### 功能开发流程
团队在使用 _gitflow_ 分支管理规范下，当我们需要有新的功能进行开发的时候，我们需要从最新的 _develop_ 分支上检出(_checkout_) 一个功能分支分支，命名形式如 _feature-xxx-version_，然后就可以在该功能分支上进行开发，当你开发完成后，可以将分支 _push_ 到远程仓库中，并在对应的远程仓库管理软件(_gitlab_ 等)中提交 _merge request_，经过相关人员的代码审查后，会将功能分支合并到 _develop_ 分支上并删除这个功能分支。
<a name="27343ef7"></a>
#### 提测流程
当开发人员完成了该版本的所有功能后，会向测试人员发起测试功能的请求，此时会将最新的 _develop_ 分支(此时的 _develop_ 分支已有所有该版本的新功能的代码)检出 _release_ 分支，命名格式如 _release-version_，测试人员 QA 会基于 _release-version_ 为主线对代码进行测试与测试环境部署。<br />当测试人员在 _release_ 分支上发布代码 _bug_ 的时候，开发人员需要从 _release-version_ 分支上检出 _bugfix_ 分支，命名格式如 _bugfix-xxx_ ，然后类似功能开发流程，将 _bugfix-xxx_ 分支合并到 _release-version_ 分支上。
<a name="b7c46109"></a>
#### 部署发布流程
当测试人员测试通过后，会将 _release-version_ 分支合并到 _master_ 分支，并将 _release-version_ 分支合并到 _develop_ 分支，将 _master_ 分支的代码进行部署，并测试线上，测试通过后对 _master_ 分支打上 _tag_，并将 _master_ 分支合并会 _develop_ 分支。这里解释一下打 tag 的作用，tag 相当于是一个当前 commit 的一个快照，作用与 commit-sha1 类似，但是 tag 更具有可读性，可以供快速回退到想要的版本记录。
<a name="f8ad7d67"></a>
#### 线上问题紧急修复流程
当线上即生产环境发现 _bug_ 的时候，需要在 master 分支上检出(_checkout_)一个 _hotfix-x.x.x_ (x.x.x 是指语义化版本号)分支，然后相关的开发人员的修复代码都合并到 _hotfix-x.x.x_ 分支上，在测试通过后，将 _hotfix-x.x.x_ 分支合并到 _master_ 与 _develop_ 分支上，并为 _master_ 分支打上 tag。
<a name="0fb65c6c"></a>
### 便于 code review 的合作流程
在编写代码的时候，为了代码的高质量与开发人员的知识共享，通常会加上代码审查也就是 code review 环节。这个环节是借助 merge request 或者 pull request 来做到的，所以我们提交的 commit 记录应该尽量保持为**一个**，这样的好处有很多：

1. 方便代码审核者进行 code review，只需要看这一个 commit 记录的逻辑即可<br />
1. 万一该 commit 的代码导致出现问题，我们可以只针对这个 commit 进行快速回退。<br />
1. 一个功能保持一个 commit 记录如果遇到需要对这个功能提前提交到某些环境比如生产环境上，我们可以快速用过 cherry-pick 命令，在对应的分支上"重现"该提交记录，达到提前提交的目的。<br />

也许你会有疑问，单个功能保持一个 commit 记录与 commit 提交记录尽量保持较细的粒度这一原则是否相悖，笔者觉得并没有冲突，因为这两个 git 协作要求是基于不同的角度来看待问题的，对于自己开发的分支上，我们需要保持每个提交的粒度在一个 commit 做一个修改，这样有利于我们记录工作内容与方便自己在本分支上做回滚，但是对于一个软件开发的主分支来说，它上面的提交应该是以功能为单位的，而无需关心这个功能内开发人员开发这个功能做了多少次修改。<br />面对这种情况，我们会使用 rebase 命令，也就是衍合(变基)操作。所谓衍合就是将你此分支上的 commit 提交，按顺序重新在某个分支上的某个基础点重新"演绎"一次，而这个重新"演绎"重新提交的 commit 记录与原来的 commit 提交会有些许不同，不同点在于 commit 的 HashId 会不同，但是提交内容是一样的。<br />rebase 命令提供了交互式的界面，并且提供多种的命令让你能够将多个 commit 记录合并为一个，从而达到我们单一功能保持一个 commit 记录的目的，保持提交历史的清爽。<br />![image.png](/8.png)<br />								(便于 code review 的 git 合作流程图)

*_注：企业中一般会使用需求管理系统来管理需要开发的需求等_

<a name="6c228def"></a>
## Git 提交规范
制定一个 _git commit_ 信息的提交规范是开发团队工作流必不可少的环节。试想一下，如果查看主分支上的历史库也就是你查看 _git log_ 的时候，打印出来的信息杂乱无章的话，如果代码遇到问题，可能需要很大的精力与成本来查找到导致问题的代码提交，所以团队需要制定规范来引导成员编写规范的 _commit_ 信息。<br />接下来的 _commit_ 信息规范参考了 _angularjs_ 团队的开发者指引与笔者的工作团队进行总结，读者如有需要可以以此为基础增加或修改成为自己团队的 _commit_ 规范的一部分。
<a name="f6e58d73"></a>
### 提交信息基本模板
如果 commit 信息都按照一定的模式进行提交，那么我们就会很容易找到自己想要的信息，模板参考如下：

```
<type>(<scope>): <subject> [<ISSUE_ID>]

<body>

<footer>
```

commit 信息包括三个字段: type (必需)， scope(可选) 和 subject(必需)。

1. type。type 是用于说明该 commit 的类型的，一般我们会规定 type 的类型如下：
  - feat: 新功能(feature)<br />
  - fix: 修复 bug<br />
  - docs: 文档(documents)<br />
  - style: 代码格式(不影响代码运行的格式变动，注意不是指 CSS 的修改)<br />
  - refactor: 重构(既不是新增功能，也不是修改 bug 的代码变动)<br />
  - test: 提交测试代码(单元测试，集成测试等)<br />
  - chore: 构建或辅助工具的变动<br />
  - misc: 一些未归类或不知道将它归类到什么方面的提交<br />
2. scope。scope 说明 commit 影响的范围，比如数据层，控制层，视图层等等，这个需要视具体场景与项目的不同而灵活变动<br />
2. subject。subject 是对于该 commit 目的的简短描述
  - 使用第一人称现在时的动词开头，比如 modify 而不是 modified 或 modifies<br />
  - 首字母小写，并且结尾不加句号<br />
4. ISSUEE_ID。这个与公司的需求管理与项目管理有关，假设你的项目放在 _github_ 上，你的需求或者 _bug_ 修复可能会有对应的 _issues_ 记录，你可以加到你的 _commit_ 信息中如 _issue-37938634_。<br />

body 其实就是 subject 的详细说明，而 footer 中你可以填写相关的需求管理 issues id。<br />在企业中一般会对团队中要做的事情与需求开发使用一个软件进行管理，好处是可以让代码与对应的用户故事(story)或者需求，bug 进行关联，便于管理，类似的方案有 github，gitlab，tracker，JIRA 等等，比如在网易某些团队中就会使用 JIRA 加上 gitlab 来进行团队管理。<br />commit message 的规范性是很重要的，对于自己养成良好的编程习惯很有帮助，但是没有必要强制完全遵循开源团队的规范，毕竟每个团队与个人的情况不同，博采众长即可，当然你也可以使用像 _commitlint_ 这样的校验工具从工具层面上来强制执行某些规范，这里就不展开讲了，有兴趣的读者可以查阅相关资料并使用到自己团队的实践中。

<a name="25f9c7fa"></a>
## 总结
本章节介绍了 _git_ 的一些常用命令的操作与基本概念，与基于此的 _git_ 合作流程与 _commit_ 规范。对于 _git_ 的使用，笔者建议多加练习，不能只"纸上谈兵", 实际操作才能加深对于 _git_ 的理解，而 _git_ 合作流程与 _commit_ 规范提供了参考，大家结合上面章节的建议与实际团队的情况，相信会有更适合每个团队不同情况的实践。
<a name="d41d8cd9"></a>
## 
<a name="fb507f2c"></a>
## 参考文献

1. _《Git 团队协作》人民邮电出版社(2017-6)_  <br />
1. _angular 团队的 git commit 规范: [https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)_<br />
<a name="d41d8cd9"></a>
## 
<a name="2e307a62"></a>
## 附录
以下是一些笔者收集的有助于练习 _git_ 的资源:

1. 一款名为 _Githug_ 的帮助练习 _git_ 操作的游戏 [https://github.com/Gazler/githug](https://github.com/Gazler/githug)<br />
1. 图文并茂的 _git_ 教程 [https://backlog.com/git-tutorial/cn/](https://backlog.com/git-tutorial/cn/)<br />
