# Git

>   参考文档：https://snailclimb.gitee.io/javaguide/#/docs/tools/Git

## 三种状态

Git 有三种状态，你的文件可能处于其中之一：

1.  **已提交（committed）**：数据已经安全的保存在本地数据库中。
2.  **已修改（modified）**：已修改表示修改了文件，但还没保存到数据库中。
3.  **已暂存（staged）**：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

由此引入 Git 项目的三个工作区域的概念：**Git 仓库(.git directoty)**、**工作目录(Working Directory)** 以及 **暂存区域(Staging Area)** 。

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4qcxem9oj30m80c90sy.jpg" alt="img" style="zoom:67%;" />

**基本的 Git 工作流程如下：**

1.  在工作目录中修改文件。
2.  暂存文件，将文件的快照放入暂存区域。
3.  提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

## 快速入门

#### 获取 Git 仓库

有两种取得 Git 项目仓库的方法。

1.  在现有目录中初始化仓库: 进入项目目录运行 `git init` 命令,该命令将创建一个名为 `.git` 的子目录。
2.  从一个服务器克隆一个现有的 Git 仓库: `git clone [url]` 自定义本地仓库的名字: `git clone [url]` directoryname

#### 记录每次更新到仓库

1.  **检测当前文件状态** : `git status`
2.  **提出更改（把它们添加到暂存区**）：`git add filename` (针对特定文件)、`git add *`(所有文件)、`git add *.txt`（支持通配符，所有 .txt 文件）
3.  **忽略文件**：`.gitignore` 文件
4.  **提交更新:** `git commit -m "代码提交信息"` （每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`）
5.  **跳过使用暂存区域更新的方式** : `git commit -a -m "代码提交信息"`。 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤。
6.  **移除文件** ：`git rm filename` （从暂存区域移除，然后提交。）
7.  **对文件重命名** ：`git mv README.md README`(这个命令相当于`mv README.md README`、`git rm README.md`、`git add README` 这三条命令的集合)

#### 推送改动到远程仓库

*   如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：`git remote add origin <server>` ,比如我们要让本地的一个仓库和 Github 上创建的一个仓库关联可以这样`git remote add origin https://github.com/xxx/test.git`

*   将这些改动提交到远端仓库：`git push origin master` (可以把 *master* 换成你想要推送的任何分支)

    如此就能够将你的改动推送到所添加的服务器上去了。

#### 远程仓库的移除与重命名

*   将 test 重命名为 test1：`git remote rename test test1`
*   移除远程仓库 test1: `git remote rm test1`

#### 查看提交历史

在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。 完成这个任务最简单而又有效的工具是 `git log` 命令。`git log` 会按提交时间列出所有的更新，最近的更新排在最上面。

**可以添加一些参数来查看自己希望看到的内容：**

只看某个人的提交记录：

```shell
git log --author=slimshady
```

#### 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交：

```shell
git commit --amend
```

取消暂存的文件

```shell
git reset filename
```

撤消对文件的修改:

```shell
git checkout -- filename
```

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：

```shell
git fetch origin
git reset --hard origin/master
```

#### 分支

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*master* 是“默认的”分支。可以在其他分支上进行开发，完成后再将它们合并到主分支上。

我们通常在开发新功能、修复一个紧急 bug 等等时候会选择创建分支。单分支开发好还是多分支开发好，还是要看具体场景来说。

创建一个名字叫做 test 的分支：

```shell
git branch test
```

切换当前分支到 test（当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。 Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样）

```shell
git checkout test
```

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf4qv777vdj30f8032q2w.jpg)

你也可以直接这样创建分支并切换过去(上面两条命令的合写)

```shell
git checkout -b test
```

切换到主分支

```shell
git checkout master
```

合并分支(可能会有冲突)

```shell
git merge test
```

把新建的分支删掉

```shell
git branch -d test
```

将分支推送到远端仓库（推送成功后其他人可见）：

```shell
git push origin 
```

