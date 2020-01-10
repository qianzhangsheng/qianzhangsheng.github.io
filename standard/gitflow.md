# Gitflow

> TODO

## 目录

- [Git Flow](/git?id=git-flow)

## Git Flow

### 简介

Gitflow 是一个 Vincent Driessen 提出的 Git 分支模型，它提供了一种基于 Git 的代码版本管理流程，规定了我们如何在项目中使用 Git。使用它，是为了简化我们的分支模型，使我们能在开发中将更少的时间花费在代码的版本管理上，同时也能避免相当多的常见问题。



---



### 我们采用的分支模型

虽然我们使用的 Gitflow 的插件中，提供了7种分支，但是我们只使用其中的5种，也是 Gitflow 的标准模型中的5种，现在，对其做一个简单的介绍。

**master 和 develop 分支是在一个项目中永久存在的分支，即不能被删除的分支。**

* master：master 是主分支，存在于 master 分支中的都是能上线的版本，所以只有在上线的时候才会将代码合并至 master 分支中。


* develop：develop 是开发分支，我们在日常开发中都是使用这个分支，所以 develop 中应该包含项目的所有版本。

**feature/`*`、release/`*`、hotfix/`*` 分支是临时分支，完成即删。**

* feature/*：feature 是功能分支，只有在开发中需要在应用中添加一个特定的功能特性时，才会去新建一个 feature 分支，我们采用的命名方式为 feature/功能名 的形式。直到该功能完全开发完成后，才会被删除，并被合并至 develop 分支中。
* release/*：release 是发布分支，只有在应用到了可以发布的阶段才会去新建一个 release 分支，我们采用的命名方式为 release/版本号 的形式。**我们做出规定，在应用进行提测的时候，须新建一个 release 分支，在测试中发现的问题都将在这个 release 分支中进行修复开发。**直到新版本发布后被删除，并被合并至 master 和 develop 分支。
* hotfix/*：hotfix 是紧急修复分支，只有在应用发布一个新版本后遇到需要紧急修复并立即发布的情况时才会使用 hotfix 分支，我们采用的命名方式为 hotfix/版本号 的形式。直到修复版本发布后被删除，并被合并至 master 和 develop 分支；若存在 release 分支，则应被合并至 release 分支和 develop 分支。



若想有更深入的了解，可参考：
http://nvie.com/posts/a-successful-git-branching-model/
http://www.oschina.net/translate/a-successful-git-branching-model?lang=chs&page=2#



---



### 我们的 Git 使用流程

不用担心，使用 Gitflow 并不会让你的日常开发变得更加繁杂，只会在一些特定的开发节点，需要我们去使用它：

* 启动/完成新版本特定功能的开发：git flow feature start/finish
* 启动/完成新版本发布（提测时）: git flow release start/finish
* 进行/完成紧急修复：git flow hotfix start/finish





**我们日常的开发中，是这样使用 Git 的：**

项目新建时：git init，git remote add ...
项目开发时：git pull，git push（git merge，git fetch，git reset，git checkout，git log，git status，git branch & etc.）



**使用 Gitflow 后：**

项目新建时：git init，git remote add ...
项目开发时：git pull，git push（git merge，git fetch，git reset，git checkout，git log，git status，git branch & etc.）

**若新增一个独立功能**：`git flow feature start <name>`
*针对这个功能的开发都会在这个分支中进行，开发中其他的部分和 bug 修复都在 develop 分支中进行。*
**完成独立功能的开发**：`git flow feature finish <name>`

**若新增一个独立功能**：`git flow feature start <name>`
*针对这个功能的开发都会在这个分支中进行，开发中其他的部分和 bug 修复都在 develop 分支中进行。*
**完成独立功能的开发**：`git flow feature finish <name>`

**当开发至能够发布时（提测）**：`git flow release start <edition> `
*在测试中发现的问题都将直接在 release 分支中修复。*
**当新版本发布时**：`git flow release finish <edition>`

**发布后需要紧急修复**：`git hotfix start <edition>`
**修复完成并发布**：`git hotfix finish <edition>`



---



### Gitflow 工具的安装及项目初始化



#### Windows

[Git for windows](https://git-for-windows.github.io/) 2.6.4 以上版本已经内置了 git-flow



#### Mac OS

使用 Homebrew：`brew install git-flow-avh`



#### 项目初始化

在终端中输入 `git flow init` ，并根据提示设置对应的分支即可。

在此，**我们做出规定**：

* 模型中 master 分支，在项目中统一命名为 master
* 模型中 develop 分支，在项目中统一命名为 dev/develop 分支
* 模型中 feature 分支，前缀为 feature/ 的形式，即分支名为 feature/name
* 模型中 release 分支，前缀为 release/ 的形式，即分支名为 feature/version
* 模型中 hotfix 分支，前缀为 hotfix/ 的形式，即分支名为 hotfix/version




---



### Gitflow 命令手册



#### feature

~~~python
# 新建一个功能分支
git flow feature start <name> [<base>]

# 完成一个功能分支
git flow feature finish <name>

# 分享一个功能分支
git flow feature publish <name>

# 追踪一个功能分支
git flow feature track <name>
~~~

更多命令及选项请参考：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-feature



#### release

~~~python
# 新建一个发布分支
git flow release start <version>

# 完成一个发布分支
git flow release finish <version>

# 分享一个发布分支
git flow release publish <name>

# 追踪一个发布分支
git flow release track <name>
~~~

更多命令及选项请参考：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-release



#### hotfix

~~~python
# 新建一个紧急修复分支
git flow hotfix start <version> [<base>]

# 完成一个紧急修复分支
git flow hotfix finish <version>

# 分享一个紧急修复分支
git flow hotfix publish <name>

# 追踪一个紧急修复分支
git flow hotfix track <name>
~~~

更多命令及选项请参考：https://github.com/petervanderdoes/gitflow-avh/wiki/Reference:-git-flow-hotfix



### 命令分解



#### feature

`git flow feature start (fly)`  →  `git checkout -b feature/(fly) dev`

`git flow feature finish (fly)`

​			     ↓

```
git checkout dev
git merge [—[no-ff|squash]] feature/(fly)
git branch -d feature/(fly)
```



#### release

` git flow release start (0.1)`  →  `git checkout -b release/(0.1) dev`

`git flow release finish (0.1)`

​			     ↓

```
git checkout master
git merge --[no-ff|squash] release/(0.1)
git tag -a v(0.1) -m "发布(0.1)版本"
git checkout dev
git merge --[no-ff|squash] release/(0.1)
git branch -d release/(0.1)
```



#### hotfix

`git flow hotfix start (0.1.1)`  →  `git checkout -b hotfix/(0.1.1) master`

`git flow hotfix finish (0.1.1)`

​			  ↓

```
git checkout master
git merge --[no-ff|squash] hotfix/(0.1.1)
git tag -a v(0.1.1) -m “发布(0.1.1)版本"
git checkout dev
git merge --[no-ff|squash] hotfix/(0.1.1)
git branch -d hotfix/(0.1.1)
```



### 已知问题



1. 当一个人 finish 一个分支时，虽然会将这一改变同步至远程仓库，但是如果有其他人 track 了这个分支，他们并不会知道这个分支已经被删除，这需要开发者之间相互通知，并及时在本地删除此分支，并切换回 debelop 分支。




### 参考资料

* Git-flow 备忘清单：http://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html






